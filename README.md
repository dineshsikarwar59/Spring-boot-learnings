# Spring-boot-questions

**Q. What is Spring Boot’s WebClient, and how does it differ from RestTemplate?**

**RestTemplate:** RestTemplate is blocking and synchronous, which means the calling thread waits for the HTTP response before moving on.
RestTemplate is more straightforward for simpler, traditional applications that don't require reactive features.

**WebClient:** WebClient is non-blocking, meaning it allows asynchronous, non-blocking calls, and is more suitable for high-performance, scalable applications. 
WebClient is ideal for reactive applications and environments where performance is crucial, particularly when making multiple concurrent HTTP requests
