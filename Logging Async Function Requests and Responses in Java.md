
In Java, when working with asynchronous operations that return `CompletableFuture` or other `Future` types, you can use several approaches to log both the request and response together. Here are the best ways:

## 1. Using a Wrapper Method (Most Flexible)

```java
import java.util.concurrent.CompletableFuture;
import java.util.function.Supplier;
import java.time.Duration;
import java.time.Instant;

public class AsyncLogger {
    public static <T> CompletableFuture<T> logAsyncCall(
            Supplier<CompletableFuture<T>> futureSupplier,
            String operationName,
            Object requestData) {
        
        Instant start = Instant.now();
        System.out.printf("[%s] Request: %s%n", operationName, requestData);
        
        return futureSupplier.get()
            .whenComplete((result, throwable) -> {
                Duration duration = Duration.between(start, Instant.now());
                if (throwable != null) {
                    System.out.printf("[%s] Failed after %s: %s%n", 
                        operationName, duration, throwable.getMessage());
                } else {
                    System.out.printf("[%s] Completed in %s: %s%n", 
                        operationName, duration, result);
                }
            });
    }
}

// Usage:
CompletableFuture<String> result = AsyncLogger.logAsyncCall(
    () -> myAsyncMethod(param1, param2),
    "myAsyncMethod",
    Map.of("param1", param1, "param2", param2)
);
```

## 2. Using CompletableFuture Wrappers

```java
public class FutureLogger {
    public static <T> CompletableFuture<T> withLogging(
            CompletableFuture<T> future,
            String operationName,
            Object requestData) {
        
        Instant start = Instant.now();
        System.out.printf("[%s] Request: %s%n", operationName, requestData);
        
        return future.whenComplete((result, throwable) -> {
            Duration duration = Duration.between(start, Instant.now());
            if (throwable != null) {
                System.out.printf("[%s] Failed after %s: %s%n", 
                    operationName, duration, throwable.getMessage());
            } else {
                System.out.printf("[%s] Completed in %s: %s%n", 
                    operationName, duration, result);
            }
        });
    }
}

// Usage:
CompletableFuture<String> future = myAsyncMethod(param1, param2);
CompletableFuture<String> loggedFuture = FutureLogger.withLogging(
    future, 
    "myAsyncMethod", 
    Map.of("param1", param1, "param2", param2)
);
```

## 3. For Spring Applications (Using AspectJ)

If you're using Spring, you can create an aspect:

```java
@Aspect
@Component
public class AsyncLoggingAspect {
    @Around("@annotation(asyncLoggable)")
    public Object logAsyncMethod(ProceedingJoinPoint joinPoint, AsyncLoggable asyncLoggable) throws Throwable {
        String methodName = joinPoint.getSignature().getName();
        Object[] args = joinPoint.getArgs();
        
        System.out.printf("[%s] Request: %s%n", methodName, Arrays.toString(args));
        Instant start = Instant.now();
        
        try {
            Object result = joinPoint.proceed();
            if (result instanceof CompletableFuture) {
                return ((CompletableFuture<?>) result).whenComplete((res, ex) -> {
                    Duration duration = Duration.between(start, Instant.now());
                    if (ex != null) {
                        System.out.printf("[%s] Failed after %s: %s%n", 
                            methodName, duration, ex.getMessage());
                    } else {
                        System.out.printf("[%s] Completed in %s: %s%n", 
                            methodName, duration, res);
                    }
                });
            }
            return result;
        } catch (Exception e) {
            Duration duration = Duration.between(start, Instant.now());
            System.out.printf("[%s] Failed after %s: %s%n", 
                methodName, duration, e.getMessage());
            throw e;
        }
    }
}

// Custom annotation
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface AsyncLoggable {
}

// Usage on your async method:
@Async
@AsyncLoggable
public CompletableFuture<String> myAsyncMethod(String param1, int param2) {
    // implementation
}
```

## Best Practices for Java

1. **Include timing information** - Use `Instant` and `Duration` for precise timing
2. **Structured logging** - Consider using JSON format with a library like Jackson
3. **Correlation IDs** - Add a unique ID to track related logs
4. **Log levels** - Use proper log levels (DEBUG, INFO, ERROR)
5. **Sensitive data** - Redact or mask sensitive information
6. **Consider MDC** - For distributed tracing in concurrent environments

The wrapper method approach (option 1) is generally the most flexible as it:
- Works with any async method
- Doesn't require framework support
- Keeps request/response logging together
- Allows for easy modification of logging behavior