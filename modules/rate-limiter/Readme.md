## Rate Limiter System Design

A rate limiter system is a tool used to control the rate at which certain actions are performed or requests are made to a system. It is used to limit the number of requests or actions within a specified time period, preventing a user or application from overwhelming the system with too many requests.

For example, let's say you have a web application that provides a search function for users. If you don't use a rate limiter system, a user can perform an unlimited number of search requests in a very short time. This can cause the system to become overwhelmed and slow down or even crash.

> A rate limiter system would prevent this by enforcing a maximum number of search requests a user can make within a certain time period. Once the limit is reached, the user would have to wait for a specified time before they can make more requests. This helps prevent abuse of the system and ensures that it remains available and responsive for all users.

---
##### API rate limiting is an important mechanism that helps ensure the stability and availability of APIs. Here are some benefits of using an API rate limiter:

* Prevents API abuse: API rate limiting can help prevent API abuse by restricting the number of requests made by a user or application within a specified time period. This helps protect the API from being overwhelmed by a small number of users, and ensures that all users have fair access to the API.

* Improves reliability and stability: By limiting the number of requests made to the API, rate limiting can prevent overloading the API, which can cause it to become unstable or unavailable. This can be especially important in high-traffic environments, where a large number of requests can quickly overwhelm the system.

* Helps manage resource utilization: By limiting the number of requests made to the API, rate limiting can help manage resource utilization, ensuring that resources are used efficiently and effectively. This can help reduce costs and improve performance.

* Provides better analytics: API rate limiting can provide better analytics and insights into API usage patterns, helping organizations identify areas where they can improve performance, reduce costs, and increase user satisfaction.

* Enables better scaling: By limiting the number of requests made to the API, rate limiting can help enable better scaling of the system. By controlling the rate at which requests are processed, the system can more easily adapt to changes in demand and avoid becoming overwhelmed.

* Enhances security: API rate limiting can help enhance security by preventing malicious users from overwhelming the system with too many requests. By limiting the number of requests made by a user or application, rate limiting can help prevent denial-of-service attacks and other types of attacks that can cause harm to the system.


### Problem Statement:

* Applications or users may send an excessive amount of requests to the system, overwhelming it and causing it to slow down or crash.
* This can lead to poor user experience, lost revenue, and potential security vulnerabilities.
* Without a rate limiter, it is difficult to control and manage the amount of traffic sent to the system.

### Design scope:

* Identify the types of requests that need to be rate limited, such as API calls, search queries, or database requests.
* Determine the rate limits for each type of request, including the maximum number of requests allowed within a specified time period.
* Decide on the algorithm for enforcing rate limits, such as token bucket, leaky bucket, or fixed window.
* Determine how to handle requests that exceed the rate limit, such as delaying or rejecting the request, or using a queuing system to process requests in a fair and efficient manner.
* Consider the impact of rate limiting on user experience and system performance, and ensure that it does not negatively impact either.
* Decide on how to monitor and measure the effectiveness of the rate limiter, such as through logging, metrics, or alerts.
* Consider how to handle edge cases and exceptions, such as requests from authenticated versus unauthenticated users, or requests from different geographic locations.
* Ensure that the rate limiter is scalable and can handle increasing levels of traffic as the system grows.
* Determine the granularity of the rate limiting, such as whether it should be applied on a per-user, per-IP address, or per-API key basis.
* Consider how to handle bursty traffic, where a user may send a large number of requests in a short period of time, followed by a period of low or no activity.
* Decide on the duration of the time period over which the rate limits will be enforced, such as per second, per minute, or per hour.
* Determine how to handle rate limiting for different tiers of users, such as free versus paid users, or for different levels of service.
* Consider how to handle rate limiting for different types of requests, such as read versus write requests, or for requests that require different levels of system resources.
* Determine how to handle rate limiting for distributed systems, where requests may be coming from multiple sources or nodes.
* Consider how to handle rate limiting for long-running requests or background tasks, which may require different rate limiting rules than short-lived requests.
* Ensure that the rate limiter is configurable and can be easily adjusted as needed to adapt to changing traffic patterns or requirements.
* Consider the impact of rate limiting on the overall system architecture and design, and ensure that it integrates smoothly with other components and services.
---
### Summary of the requirements for the system

* **Accurately limit excessive requests:**

- * The rate limiter should be able to accurately track and enforce the allowed rate limit for each type of request.
- * The rate limiter should be able to handle different rates for different types of requests or users.
- * The rate limiter should be able to prevent users from circumventing the limit by using multiple IP addresses or API keys.

* **Low latency:**

- * The rate limiter should not introduce any significant delay or impact on the HTTP response time.
- * The rate limiter should be able to process requests quickly and efficiently.
- * The rate limiter should be able to handle a large number of requests in a short amount of time without slowing down the system.

* **Use as little memory as possible:**

- * The rate limiter should be designed to use minimal memory resources.
- * The rate limiter should be able to handle a large number of requests without consuming excessive memory.

* **Distributed rate limiting:**

- * The rate limiter should be able to handle rate limiting across multiple servers or processes.
- * The rate limiter should be designed to be scalable and able to handle increasing levels of traffic.
- * The rate limiter should be able to synchronize the rate limit across all instances in a distributed system.

* **Exception handling:**

- * The rate limiter should provide clear exceptions to users when their requests are throttled.
- * The rate limiter should be able to provide useful error messages and status codes that can be easily understood by users and developers.
- * The rate limiter should be able to handle different types of exceptions, such as server errors, network timeouts, or other issues that may arise.

* **High fault tolerance:**

- * The rate limiter should be designed to be fault-tolerant and able to handle errors or failures in the system.
- * The rate limiter should be able to recover from errors or failures and continue to function without affecting the entire system.
- * The rate limiter should be able to detect and handle problems with cache servers or other components that may be used in the system.

---

#### Client-side rate limiter:
A client-side rate limiter is implemented within the client application or library and is responsible for controlling the number of requests that are sent to the server. This can help prevent the client from overwhelming the server with too many requests and can also help reduce the amount of data that needs to be transferred over the network.

The client-side rate limiter is typically implemented by adding a delay between each request or by queuing requests and sending them at a controlled rate. This can be useful in cases where the server does not provide any rate limiting or when the client wants to control the rate of requests independently of the server.

#### Server-side rate limiter:
A server-side rate limiter is implemented on the server and is responsible for controlling the rate of requests that are accepted from clients. The server-side rate limiter is typically used to prevent clients from overwhelming the server with too many requests and to protect against denial of service attacks.

The server-side rate limiter can be implemented in various ways, such as using a token bucket algorithm, sliding window algorithm, or leaky bucket algorithm. It typically tracks the number of requests made by each client and enforces a limit on the number of requests that can be made within a certain time period.

The server-side rate limiter can be applied globally to all clients or can be applied on a per-client basis, depending on the requirements of the application. It can also be combined with other security measures, such as authentication and authorization, to provide a comprehensive security solution.


In a microservices architecture, where APIs are exposed by multiple services, implementing a rate limiter in an API gateway is a common approach. This allows for centralized management and enforcement of API usage policies across all services.

Implementing the rate limiter in the API gateway provides several advantages:

* **Centralized control:** The API gateway can provide a centralized location for enforcing rate limits across all services, making it easier to manage and control API usage policies.

* **Simplified architecture:** Implementing the rate limiter in the API gateway simplifies the architecture of the microservices, as each service does not need to implement its own rate limiter.

* **Scalability:** The API gateway can be horizontally scaled to handle increased traffic and enforce rate limits for all services.

* **Security:** The API gateway can provide additional security features, such as SSL termination, authentication, and IP whitelisting.

* **Improved performance:** By implementing rate limiting in the API gateway, the individual microservices can focus on their core functionalities, leading to improved performance.

> There may be cases where implementing a rate limiter on the server-side makes more sense, such as when rate limiting is specific to a particular service or when the service needs to enforce more complex rate limiting rules. Ultimately, the decision of where to implement the rate limiter will depend on the specific needs and requirements of the system.

---
### Algorithms for rate limiting

* **Token Bucket Algorithm:** The token bucket algorithm is a simple algorithm where a token is added to a bucket at a fixed rate. Each request that comes in to the system removes a token from the bucket. If the bucket is empty, the request is rejected. This algorithm provides a smooth and constant rate of requests over time.

* **Leaky Bucket Algorithm:** The leaky bucket algorithm is another simple algorithm where requests are queued in a bucket. The bucket has a fixed capacity and can only hold a certain number of requests. Requests are removed from the bucket at a fixed rate, regardless of whether they are serviced or not. This algorithm can help prevent sudden bursts of traffic and smooth out the rate of requests over time.

* **Sliding Window Algorithm:** The sliding window algorithm is a more complex algorithm that uses a sliding window to track the rate of requests over time. The algorithm divides time into fixed intervals and counts the number of requests made in each interval. If the number of requests exceeds a predefined limit, the algorithm rejects the request. This algorithm provides more fine-grained control over the rate of requests and can help prevent bursts of traffic.

* **Adaptive Rate Limiting Algorithm:** The adaptive rate limiting algorithm is a more advanced algorithm that adjusts the rate of requests based on the current state of the system. It takes into account the current load on the system and adjusts the rate of requests dynamically. This algorithm can provide more accurate and efficient rate limiting, but it is more complex to implement.

---

### Token Bucket Algorithm

The Token Bucket Algorithm is a simple and widely used algorithm for rate limiting. It operates by adding tokens to a "bucket" at a fixed rate and removing tokens from the bucket for each request made to the system. If there are no tokens available in the bucket, the request is rejected. Here's how it works:

* The token bucket starts with a fixed number of tokens in it, which represents the maximum number of requests that can be made in a certain time period (e.g., 100 requests per minute).

* Tokens are added to the bucket at a fixed rate (e.g., 10 tokens per second). This means that the bucket will refill at a rate of 10 tokens per second until it reaches its maximum capacity.

* When a request comes in, a token is removed from the bucket. If there are no tokens left in the bucket, the request is rejected.

* If the bucket has enough tokens to fulfill the request, the token is removed from the bucket, and the request is processed.

* If the request is processed successfully, the system waits for a fixed period of time (e.g., 0.1 seconds) before allowing the next request to come in. This ensures that requests are spaced out evenly over time.

* If the bucket is full and no additional tokens can be added, any tokens that are added after this point will be discarded.


> Here's an example of how the token bucket algorithm works:

Let's say we want to limit the number of requests to our API to 100 requests per minute. We can set up a token bucket with a maximum capacity of 100 tokens. We'll add tokens to the bucket at a rate of 10 tokens per second.

At the start of each minute, the bucket is full with 100 tokens. When a request comes in, we remove a token from the bucket. If the bucket is empty, the request is rejected. If the bucket has tokens left, the request is processed and the token is removed from the bucket.

For example, let's say we receive 20 requests in the first second. Each request removes one token from the bucket, leaving us with 80 tokens. We can continue to process requests as long as there are tokens in the bucket. However, if we receive more than 100 requests in a minute, we will start rejecting requests until the bucket has tokens available again.

##### The token bucket algorithm provides a simple and effective way to limit the rate of requests to a system while maintaining low latency and consistent performance.

##### Token Bucket Algorithm in Java

```java
public class TokenBucket {
    private final int capacity;
    private final double refillRate;
    private double tokens;
    private Instant lastRefillTime;

    public TokenBucket(int capacity, double refillRate) {
        this.capacity = capacity;
        this.refillRate = refillRate;
        this.tokens = capacity;
        this.lastRefillTime = Instant.now();
    }

    public synchronized boolean tryConsume(int tokensRequested) {
        refill();
        if (tokens >= tokensRequested) {
            tokens -= tokensRequested;
            return true;
        } else {
            return false;
        }
    }

    private void refill() {
        Instant now = Instant.now();
        double timeSinceLastRefill = Duration.between(lastRefillTime, now).toNanos() / 1_000_000_000.0;
        double tokensToAdd = timeSinceLastRefill * refillRate;
        tokens = Math.min(tokens + tokensToAdd, capacity);
        lastRefillTime = now;
    }
}
```

> This implementation defines a `TokenBucket` class with a capacity representing the maximum number of tokens that the bucket can hold and a refillRate representing the rate at which tokens are added to the bucket. The tokens field keeps track of the current number of tokens in the bucket, and the lastRefillTime field keeps track of the last time the bucket was refilled.

> The `tryConsume` method attempts to consume a certain number of tokens. It first calls the refill method to refill the bucket with any tokens that have been added since the last refill. If there are enough tokens in the bucket to satisfy the request, the method subtracts the requested number of tokens from the bucket and returns true. Otherwise, the method returns false.

> The `refill` method calculates the time since the last refill and adds tokens to the bucket at a rate determined by the `refillRate`. The number of tokens added is capped at the capacity of the bucket to prevent it from overflowing.

> You can use this class to implement rate limiting in your application by creating a `TokenBucket` instance with your desired `capacity` and `refillRate`, and calling the `tryConsume` method before processing each request. If the method returns false, you can reject the request or delay it until more tokens become available.

---
### Leaky Bucket Algorithm

The Leaky Bucket Algorithm is another rate limiting algorithm that operates by limiting the rate at which requests are processed. It is conceptually similar to the Token Bucket Algorithm, but instead of using a fixed number of tokens, it uses a "leaky bucket" that can hold a limited amount of water. The bucket has a small hole in the bottom, and water is poured into the bucket at a fixed rate. If the bucket fills up and water continues to pour in, the excess water spills out of the hole in a steady stream until the bucket is empty again.

In the context of rate limiting, the "water" in the bucket represents requests, and the "hole" in the bottom of the bucket represents the maximum rate at which requests can be processed. If requests arrive faster than the maximum processing rate, they are "spilled" or dropped.

Here's how the Leaky Bucket Algorithm works in more detail:

1. Initialize a counter to 0.
2. For each incoming request:
- 1. Wait until the counter is at or below a certain threshold (the "bucket size").
- 2. Add the request to the counter.
- 3. Process the request.
- 4. Subtract a fixed amount from the counter every fixed time interval (the "leak rate").

> The leak rate controls the rate at which the bucket "leaks" requests, which in turn controls the maximum rate at which requests can be processed. The bucket size determines the maximum number of requests that can be held in the bucket at any given time.

##### Example of Token Bucket Algorithm in Java:

```java
public class LeakyBucket {
    private long maxBucketSize;
    private long currentBucketSize;
    private long lastLeakTime;
    private double leakRate;

    public LeakyBucket(long maxBucketSize, double leakRate) {
        this.maxBucketSize = maxBucketSize;
        this.currentBucketSize = 0;
        this.lastLeakTime = System.currentTimeMillis();
        this.leakRate = leakRate;
    }

    public synchronized boolean tryConsume(long numTokens) {
        leak();
        if (currentBucketSize + numTokens > maxBucketSize) {
            return false;
        } else {
            currentBucketSize += numTokens;
            return true;
        }
    }

    private void leak() {
        long now = System.currentTimeMillis();
        double tokensToLeak = (now - lastLeakTime) * leakRate / 1000;
        currentBucketSize = Math.max(0, currentBucketSize - (long)tokensToLeak);
        lastLeakTime = now;
    }
}
```

* `LeakyBucket(long maxBucketSize, double leakRate)`: This is the constructor of the class which takes in two parameters - maxBucketSize which is the maximum size of the bucket and leakRate which is the rate at which the bucket leaks tokens.
* `tryConsume(long numTokens)`: This method is used to check if a given number of tokens can be consumed from the bucket. It takes in one parameter - numTokens which is the number of tokens to be consumed. If the bucket has enough tokens to fulfill the request, it returns true and reduces the current size of the bucket by numTokens. Otherwise, it returns false.
* `leak()`: This method is called internally by the tryConsume method to leak tokens from the bucket. It calculates the time since the last leak and the number of tokens to be leaked based on the leak rate. It then updates the current size of the bucket by subtracting the leaked tokens.
The LeakyBucket class uses a synchronized block to ensure thread-safety while updating the current size of the bucket. The System.currentTimeMillis() method is used to calculate the time since the last leak or refill, and the Math.max and Math.min methods are used to ensure that the current size of the bucket does not exceed the maximum size or go below zero.

---
### Sliding Window Algorithm

The Sliding Window Algorithm is another technique used for rate limiting. It is based on dividing time into small intervals or windows and counting the number of requests made during each interval. The sliding window refers to the continuous shift of the window over time. Here are the details of how the algorithm works:

* **Dividing time into small intervals:** We divide the time into small intervals of equal length, for example, one second intervals. Each interval is called a window.

* **Counting requests within each window:** We maintain a counter for each window that keeps track of the number of requests made within that window. We can use a data structure like an array or a hash map to store these counters.

* **Sliding the window:** As time progresses, we slide the window over time. At every time interval, we slide the window by one position and update the counter for the new window.

* **Throttling requests:** We can use a threshold value to limit the number of requests that can be made within a window. If the number of requests exceeds the threshold value, we can reject the request or delay it.

* **Resetting the window:** We can reset the counter for each window after a fixed interval, for example, every minute.

* **Handling edge cases:** We need to handle edge cases like requests that span across multiple windows or requests that arrive at the same time.

> Here is an example to illustrate the Sliding Window Algorithm:

Suppose we want to limit the rate of requests to 10 requests per second. We can use one-second windows and maintain a counter for each window. Initially, all counters are set to zero.

```
[0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
```

When a request arrives, we check the counter for the current window. If the counter is less than the threshold value, we increment the counter and allow the request. Otherwise, we reject or delay the request.

Suppose we receive 12 requests in the first second. We increment the counter for the first window by 12.

```
[12, 0, 0, 0, 0, 0, 0, 0, 0, 0]
```

In the second second, we slide the window and update the counter for the new window.

```
[0, 12, 0, 0, 0, 0, 0, 0, 0, 0]
```

Suppose we receive 8 requests in the second second. We increment the counter for the second window by 8.

```
[0, 8, 0, 0, 0, 0, 0, 0, 0, 0]
```

> If we receive more than 10 requests in any second, we reject or delay the excess requests. If we receive requests that span across multiple windows, we count them in each window. If we receive requests that arrive at the same time, we can use a random or round-robin algorithm to distribute them across the windows.

###### The Sliding Window Algorithm is a simple and effective technique for rate limiting. It can handle both short-term and long-term bursts of requests, and it can adapt to changing traffic patterns. However, it may require more memory to store the counters for each window, and it may not be suitable for low-latency applications where delays are not acceptable.

#### Sliding Window Algorithm in Java:

```java
import java.util.LinkedList;

public class SlidingWindowRateLimiter {
    private final LinkedList<Long> requests;
    private final int windowSize;
    private final int maxRequests;
    private final long timeUnit;
    private final long timeUnitInMillis;

    public SlidingWindowRateLimiter(int windowSize, int maxRequests, long timeUnit) {
        this.requests = new LinkedList<>();
        this.windowSize = windowSize;
        this.maxRequests = maxRequests;
        this.timeUnit = timeUnit;
        this.timeUnitInMillis = timeUnit * 1000;
    }

    public synchronized boolean allowRequest() {
        long currentTime = System.currentTimeMillis();
        long startTime = currentTime - (windowSize * timeUnitInMillis);

        if (requests.isEmpty() || requests.getLast() < startTime) {
            requests.addLast(currentTime);
            return true;
        }

        if (requests.size() < maxRequests) {
            requests.addLast(currentTime);
            return true;
        }

        long oldestRequestTime = requests.getFirst();
        if (oldestRequestTime >= startTime) {
            return false;
        }

        requests.removeFirst();
        requests.addLast(currentTime);

        return true;
    }
}
```

In this implementation, we use a LinkedList to keep track of the request timestamps within the sliding window. The windowSize parameter specifies the size of the sliding window in seconds, while the `maxRequests` parameter specifies the maximum number of requests allowed within that window. The timeUnit parameter specifies the time unit used for the window size (e.g. seconds, minutes, etc.).

The `allowRequest()` method checks if a request is allowed based on the sliding window. If the list of requests is empty or the last request occurred before the start of the current sliding window, the request is allowed and its timestamp is added to the list. If the list has not reached the maximum number of requests, the request is allowed and its timestamp is added to the list. Otherwise, if the first request in the list occurred before the start of the current sliding window, that request is removed and the new request is allowed and its timestamp is added to the list. If the first request in the list occurred within the current sliding window, the new request is not allowed.

This implementation is thread-safe, as we synchronize the `allowRequest()` method to prevent race conditions between multiple threads accessing the same rate limiter instance.

---
### Adaptive Rate Limiting Algorithm

Adaptive Rate Limiting is a more advanced rate limiting algorithm that takes into account the varying traffic patterns of a system and adapts the rate limiting algorithm accordingly. It aims to maintain the desired level of service quality while preventing service overloads.

The Adaptive Rate Limiting Algorithm involves the following steps:

* Monitor the request traffic: The first step in the Adaptive Rate Limiting Algorithm is to monitor the incoming request traffic to the system. This is usually done by tracking the number of requests per unit of time, such as requests per second.

* Analyze the traffic pattern: The system needs to analyze the request traffic to determine the pattern of requests. This helps in identifying any patterns of high or low traffic volume, peak hours of usage, etc.

* Adjust the rate limiter: Based on the analysis of the traffic pattern, the rate limiter needs to be adjusted accordingly. For example, during periods of high traffic, the rate limiter can be set to allow fewer requests, while during periods of low traffic, it can be set to allow more requests. This helps to maintain the desired level of service quality while preventing service overloads.

* Implement a feedback loop: The Adaptive Rate Limiting Algorithm uses a feedback loop to continuously monitor the traffic pattern and adjust the rate limiter accordingly. This helps to ensure that the rate limiter is always optimized for the current traffic pattern.

* Adaptive Rate Limiting Algorithm is more complex than other rate limiting algorithms and requires more resources to implement. However, it provides more fine-grained control over the rate limiting process and can help prevent service overloads during periods of high traffic.

##### The following are some of the benefits of using Adaptive Rate Limiting Algorithm:

* **Increased reliability:** Adaptive Rate Limiting Algorithm helps to ensure that the system is always responsive to user requests, even during periods of high traffic.

* **Improved user experience:** By preventing service overloads and maintaining a consistent level of service quality, Adaptive Rate Limiting Algorithm can help improve the user experience.

* **Better resource utilization:** By optimizing the rate limiter based on the traffic pattern, Adaptive Rate Limiting Algorithm can help improve resource utilization and reduce waste.

* **Scalability:** Adaptive Rate Limiting Algorithm can be easily scaled to handle larger volumes of traffic without compromising service quality.


> Implementing Adaptive Rate Limiting Algorithm is a complex task that requires careful analysis of the traffic patterns and setting appropriate rate limits dynamically

```java
import java.util.concurrent.TimeUnit;

public class AdaptiveRateLimiter {

    private final int MAX_WINDOW_SIZE = 1000; // Maximum number of requests allowed per second
    private final int MIN_WINDOW_SIZE = 100; // Minimum number of requests allowed per second
    private final long TARGET_LATENCY = TimeUnit.MILLISECONDS.toNanos(200); // Target latency in nanoseconds
    private final long DECAY_FACTOR = TimeUnit.MILLISECONDS.toNanos(1000); // Decay factor in nanoseconds
    private final long[] requestTimes = new long[MAX_WINDOW_SIZE];
    private int windowSize = MAX_WINDOW_SIZE;

    public boolean allowRequest() {
        long currentTime = System.nanoTime();
        int index = (int) (currentTime % MAX_WINDOW_SIZE);
        long oldestRequestTime = requestTimes[index];
        long elapsedTime = currentTime - oldestRequestTime;
        double rate = (double) MAX_WINDOW_SIZE / (double) windowSize;
        double latency = (double) elapsedTime / (double) windowSize;
        if (latency < TARGET_LATENCY) {
            windowSize = Math.min(MAX_WINDOW_SIZE, (int) (windowSize * Math.exp((latency - TARGET_LATENCY) / DECAY_FACTOR)));
        } else {
            windowSize = Math.max(MIN_WINDOW_SIZE, (int) (windowSize / Math.exp((latency - TARGET_LATENCY) / DECAY_FACTOR)));
        }
        requestTimes[index] = currentTime;
        return (Math.random() < rate);
    }
}
```

In this example, we define a sliding window of fixed size MAX_WINDOW_SIZE to track the incoming requests. The requestTimes array is used to store the timestamps of the incoming requests. We also define a target latency TARGET_LATENCY in nanoseconds and a decay factor DECAY_FACTOR in nanoseconds.

The `allowRequest` method is called for each incoming request. It first calculates the elapsed time since the oldest request in the window and the rate of incoming requests. Then, it calculates the current latency of the system and adjusts the window size based on the latency. If the latency is below the target latency, the window size is increased, and if the latency is above the target latency, the window size is decreased. Finally, the current request timestamp is stored in the `requestTimes` array, and the method returns `true` or `false` based on whether the request should be allowed or not, using a random probability based on the current rate.