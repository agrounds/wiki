# (Truely) Parallel Computing

The `grpc` library for Python has some strange quirks which make multiprocessing rather fraught. Quoting [the official `grpc` examples repo](https://github.com/grpc/grpc/tree/master/examples/python/multiprocessing):

>The library is implemented as a C extension, maintaining much of the state that drives the system in native code. As such, upon calling [`fork`](http://man7.org/linux/man-pages/man2/fork.2.html), any threads in a critical section may leave the state of the gRPC library invalid in the child process. See this [excellent research paper](https://www.microsoft.com/en-us/research/uploads/prod/2019/04/fork-hotos19.pdf) for a thorough discussion of the topic.
>
>Calling `fork` without `exec` in your process _is_ supported before any gRPC servers have been instantiated. Application developers can take advantage of this to parallelize their CPU-intensive operations.

## A Fleshed-Out Example

Below is a simple `grpc` server which accepts requests in a single process and offloads CPU-intensive work to multiple worker processes via a thread-safe queue. It receives completion updates from the worker processes via another queue and makes those available to consumers via a streaming RPC.

**server.py**
```python
import asyncio

import grpc
from google.protobuf import empty_pb2
from google.protobuf.json_format import MessageToDict

from com.agrounds.dosomething.v1 import do_something_pb2_grpc


class DoSomethingServiceServicer(do_something_pb2_grpc.DoSomethingServiceServicer):
    def __init__(self, work_queue, update_queue):
        self.work_queue = work_queue
        self.update_queue = update_queue

    async def doSomething(self, request, context):
        try:
	        for item in request:
	            self.work_queue.put(MessageToDict(item))
        except Exception:
            # handle errors
            pass
        finally:
	        return empty_pb2.Empty()

    async def getUpdates(self, request, context):
        while True:
            try:
                while not self.update_queue.empty():
                    update = self.update_queue.get()
                    yield turn_into_grpc_message_somehow(update)
	        except Exception:
	            # handle errors
	            pass
            finally:
                # add delay so we don't use the whole CPU while queue is empty
                await asyncio.sleep(0.1)


def serve(work_queue, update_queue):
    async def run_server():
        server = grpc.aio.server()
        do_something_pb2_grpc.add_DoSomethingServiceServicer_to_server(
            DoSomethingServiceServicer(work_queue, update_queue), server)
        server.add_insecure_port(f'[::]:8080')
        await server.start()
        await server.wait_for_termination()
    # above wrapper function makes it easy to ensure we only call this once
    # in this process
	asyncio.run(run_server())
```

**worker.py**
```python
import asyncio

from somelib import cpu_intensive_work


def worker(work_queue, update_queue):
    async def do_work():
        while True:
            # blocks this process until we get something to work on
            item = work_queue.get()
            result = await cpu_intensive_work(item)
            update_queue.put(result)
    # above wrapper function makes it easy to ensure we only call this once
    # in this process
    asyncio.run(do_work())
```

**main.py**
```python
import multiprocessing
from multiprocessing import Queue

from worker import worker
from server import serve


MAX_SIZE = 10000


def main():
    work_queue = Queue(maxsize=MAX_SIZE)
    update_queue = Queue(maxsize=MAX_SIZE)
    process_count = multiprocessing.cpu_count()
    processes = []
    for i in range(process_count):
        if i == 0:
            p = multiprocessing.Process(
                target=serve,
                args=(work_queue, update_queue)
            )
        else:
            p = multiprocessing.Process(
                target=worker,
                args=(work_queue, update_queue)
            )
        # marking the process as daemon means it cannot spawn child
        # processes, and it will be killed when the parent process ends
        p.daemon = True
        p.start()
        processes.append(p)

    for p in processes:
        p.join()


if __name__ == '__main__':
    main()
```