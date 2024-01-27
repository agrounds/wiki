---
title: Coroutines Cheatsheet
---
# Parallel Jobs, Tolerating Failure

Example: we've got several requests we need to make to another service. Each is for a task that is independent of the others, so even if one request fails we'd like the others to be tried. However, if one or more fail we'd like to log the failures and throw at least one of them so it's reflected to the caller of our service and our logging system.

```kotlin
private val logger = KotlinLogging.logger {}

class CustomContext(val usefulId: String) : CoroutineContext.Element {
    override val key: CoroutineContext.Key<*> = Key
    companion object Key : CoroutineContext.Key<CustomContext>
}

suspend fun main() {
    val ids = listOf("id1", "id2", "id3")

    // CopyOnWriteArrayList is a thread-safe list implementation
    val errors = CopyOnWriteArrayList<Pair<CustomContext?, Throwable>>()
    val exceptionHandler = CoroutineExceptionHandler { context, throwable ->
        errors.add(context[CustomContext] to throwable)
    }
    supervisorScope {  // does not cancel children if one fails
        ids.forEach { id ->
            launch(coroutineContext + CustomContext(id) + exceptionHandler) {
                callExternalService(id)
            }
        }
    }
    // log all errors and throw the first one
    if (errors.isNotEmpty()) {
        errors.forEach { (context, error) ->
            logger.error(error) {
                "Error while calling external service. Id = ${context?.usefulId}"
            }
            throw errors.first().let { (_, error) -> error }
        }
    }
}

suspend fun callExternalService(usefulId: String) {
    // make some HTTP call, throwing an exception if it fails
}
```

#kotlin #coroutines