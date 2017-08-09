This project is to demonstrate fault tolerance and self healing in a cloud native application. It demonstrates the Failures such as cascading failures in a distributed system and the circuit breaker pattern to handle such failures. 

In a typical production environment, failures can be due to a failure in a hardware or in the network or in the software. The chance of failures in a distributed system gets multiplied with any of such failures.

In order to handle such failures we need to tolerate the failures and gracefully degrade. Another way to handle such failures is by limiting the number of resources being consumed. 

The first strategy for fault tolerance is a well known pattern called "Circuit Breaker Pattern". It's a design pattern which is used to detect failures and encapsulates logic of preventing a failure to reoccur constantly. 

Spring Cloud implements Fault Tolerance with the help of a library called Netflix Hystrix. Hystrix is a latency and fault tolerance library designed to stop cascading failure and enable resilience in a complex distributed system where failure is inevitable. Its a concrete implementation of the circuit breaker pattern that allows a programmer to easily wrap calls and automatically watches them for failures that meet a certain volume and an error percentage threshold within a given rolling window. The default for the rolling window is 10 seconds and the request volume must be at least 20 requests.
If the error percentage is equal to or greater than 50% then the circuit will be tripped and no further requests would be allowed, any further. Hystrix will periodically recheck if the circuit should be closed. It does that by allowing a single request through every 5 seconds. If that request  succeeds then it would close the circuit and if it fails then the circuit will remain open. Any requests that are short cicuited or timed out or failed or rejected, will be given a chance to execute a fallback method. A fallback might be something returning cached data or a default value.

Hystrix has an additional functionality that protects services from being overloaded. All Hystrix wrap calls are bounded either by a Thread pool or a semaphore, to constrain the resource usage.

In the Application class add an annotation "EnableCircuitBreaker" and in either @Component or @Service class, locate the method that needs to be wrapped with hystrix and add @HystrixCommand(fallbackMethod="myFallbackMethod"). The Hystrix timeout should encompass the caller timeouts, retries and a little bit of buffer. The default timeout is set to 1000 ms. In order to set the timeout, we need to set the property hystrix.command.default.execution.isolation.thread.timeoutInMilliseconds=<timeout_ms>

In order to quickly bootstrap an discovery server application, go to http://start.spring.io/ and use the Spring Initializr to stub it out.
Add the dependencies Eureka Server, Hystrix, Actuator and the Web.

