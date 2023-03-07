This project provides declarative retry support for Symfony applications.

### Installation

### Annotation example

```php
use Symfony\Bundle\Retry\Annotations\Retryable;
use Symfony\Bundle\Retry\Annotations\Backoff;

class AppService {
    /**
     * @Retryable(
     *      retryFor="\LogicException",
     *      noRetryFor="\InvalidArgumentException",
     *      maxAttempts=5,
     *      delay=100, 
     *      multiplier=2
     * )
     */
    public function retryableMethod($arguments) {
    
    }
}
```

### Arguments
#### retryFor
Exception types that are retryable. Defaults to empty (and, if noRetryFor is also empty, all \Throwable are retried)

#### noRetryFor
Exception types that are not retryable. Defaults to empty (and, if retryFor is also empty, all exceptions are retried).

#### maxAttempts
The maximum number of attempts (including the first failure), defaults to 3.

#### backoff
Specify the backoff properties for retrying this operation.

#### delay 
Delay in milliseconds between retries.

#### multiplier
Positive integer used as a multiplier to generate the next delay value.

`@Backoff(delay=100, multiplier=2)` with `maxAttempts=4` will execute as following assuming that retryableMethod 3 times:

````php
    -> call retryableMethod (1st attempt)
        -> wait 100 ms (initial delay value)
    -> call retryableMethod (2nd attempt)
        -> wait 200 ms (100x2)
    -> call retryableMethod (3rd attempt)
        -> wait 400 ms (200x2)
    -> call retryableMethod (4th attempt)
        -> return value to original caller or throw the caught exception
````
