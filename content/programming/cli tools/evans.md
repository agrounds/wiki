https://github.com/ktr0731/evans

This is the best `grpc` client I've found so far.

# Formatting JSON Requests

`evans` allows you to call RPCs using its CLI mode, with request messages formatted in JSON. For example (host, package, service details omitted)

```shell
echo '{"myfield": "myvalue"}' | evans cli call someRPC
```

There are at least two cases in which `evans` expects a simpler version of a value in place of the "real" message type.
### StringValue
Use a regular string for this instead of an object like `{"value": "yourstring"}`.
### Timestamp
Even though the `google.protobuf.Timestamp` type is really a message with seconds and nanos as fields, `evans` expects an ISO-8601 formatted string, e.g.
```json
{"mytimestamp": "2024-01-24T18:49:24Z"}
```

On  MacOS, you can generate a string in this format using
```shell
date -u +"%Y-%m-%dT%H:%M:%SZ"
```