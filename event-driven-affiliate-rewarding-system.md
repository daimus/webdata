# Event Driven Affiliate and Rewarding System

This project is essentially an e-commerce platform, but the client wanted to integrate an affiliate and rewarding system to attract more buyers. Building the e-commerce functionality itself is straightforward, but handling affiliate and reward commissions efficiently is challenging. Traditional approaches can lead to high latency and a higher risk of failures.

To tackle this, the project is built with Node.js for the backend, React.js for the frontend, and MySQL as the database. While this tech stack is not overly complex, it provides a cost-effective solution that allows the client to generate up to 3 Billion IDR per month—an impressive result on a budget-friendly setup.

## Synchronous Processing Hurts User Experience

Calculating affiliate and reward commissions is a multi-step process that requires checking various criteria before issuing a commission. Calling the commission calculation function directly is not feasible, as the process can take up to 5 seconds, causing delays for users waiting for their transaction response. To enhance user experience, I needed a way to speed up this process.

Initially, I considered parallel processing and scaling up server resources. However, I realized that the bottleneck wasn’t hardware limitations but the fact that the calculation was being processed synchronously—each step depended on the previous one, preventing true parallelism.

The solution was to process the user’s transaction first and then trigger the commission calculation asynchronously in the background. This approach significantly improved transaction speed, ensuring a seamless user experience while maintaining accurate commission calculations.

## Managing Asynchronous Processes

JavaScript has libraries like *event-dispatch* that can handle event-driven processing. While this library could work, I wanted a more controllable solution—one that allows me to debug issues, observe logs, and rerun events if necessary. After evaluating different options, it boils down to:

- RabbitMQ
- Bull (Redis-based queue)

This tech stack allows me to publish events and process them via subscribers efficiently. Both tools serve different purposes but complement each other well.

RabbitMQ is a pure event-driven system with a dashboard to track events, inspect event data, and manually trigger events when needed.

Bull is primarily a job queue handler but also supports event-driven processing. Its advantage is its Redis integration, which allows me to store and retrieve queued tasks, implement rate limiting and retries, use Redis as a caching layer to speed up processing.

With this setup, I can ensure reliable and scalable asynchronous processing while maintaining full visibility and control over event execution.

## Mitigating Calculation Errors

Commission calculation consists of multiple steps, and failures at any point in the process can lead to significant issues. One of the biggest concerns is that an error in a single step might affect the entire payout. In some cases, commissions might not be paid at all, while in a worse scenario, only some commissions from a set of transactions get processed. If this happens, it becomes a nightmare for developers—not only do they need to identify and fix the root cause of the issue, but they must also carefully determine which users have already received their commission to avoid duplicate payments.

To mitigate this risk, I implemented database transactions to ensure data integrity. With this approach, if any part of the calculation process fails, the entire set of database operations is rolled back. This means that rather than having an inconsistent state where only some commissions are paid, the system ensures that no commission is processed at all until all conditions are met successfully. While this might seem like a harsh approach, it is far better than dealing with partial payouts, which can lead to financial discrepancies and a loss of trust from affiliates.

The impact of this solution is significant. If an error occurs, I can reproduce the event in a controlled development environment, diagnose the issue, apply the necessary fixes, and then simply retrigger the failed event without worrying about overpaying or underpaying commissions. This structured approach prevents duplicate payments and guarantees that commissions are only processed once all conditions are met correctly. With this method, I can maintain a robust, reliable, and easily debuggable commission system, ensuring smooth operation without unexpected financial inconsistencies.

## Conclusion

Through this project, I learned that choosing the right methodology is just as important as choosing the right technology. While the tech stack I used—Node.js, React.js, MySQL, RabbitMQ, and Bull (Redis)—is relatively simple, the key to success was how I implemented these tools effectively to solve complex problems.

One of the biggest challenges was ensuring that commission calculations were accurate and efficient. A synchronous approach was not an option, as it would slow down user transactions. By implementing an event-driven architecture, I was able to decouple commission calculations from real-time transactions, significantly improving response times and make code well-structured.

Another critical lesson was the importance of data integrity in financial transactions. By leveraging database transactions, I ensured that commission payouts were processed reliably—either all commissions were paid correctly, or none at all, preventing partial payments and duplicate payouts. This approach made debugging and recovery much more manageable, allowing failed events to be retriggered without financial inconsistencies.

Ultimately, this project proves that a billion-rupiah business doesn’t require a complex tech stack—just the right approach to problem-solving. With well-structured asynchronous processing, proper error handling, and a scalable event-driven system, even a budget-friendly setup can handle high transaction volumes efficiently and reliably. 🚀