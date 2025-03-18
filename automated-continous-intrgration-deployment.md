# Automated Continous Integration & Deployment

This project is a Freight Forwarding System designed to streamline logistics operations. The system consists of multiple backend services and a centralized back-office frontend, all built using .NET. In this project is integrating two different infrastructures—a self-managed environment and AWS—to create deployment strategy.

My role in this project focuses on managing the Continuous Integration and Continuous Deployment (CI/CD) pipeline to ensure seamless building and delivery of backend services and NuGet packages across a global edge network.

## Delivering Service

Delivering backend services and the back-office frontend in this project is a straightforward yet well-structured process. To achieve this, I have set up Jenkins as the primary tool for building, packaging, and deploying services. By leveraging Jenkins, the system can automatically detect changes in the codebase, trigger builds, run tests, and deploy new versions to the target infrastructure. This ensures that all services remain stable, up-to-date.

Jenkins plays a crucial role in automating the CI/CD pipeline, ensuring that every update to the backend services or the frontend is properly built, tested, and deployed with minimal manual intervention.

The CI/CD pipeline I set up is primarily designed for development and testing purposes, meaning that deployment frequency is very high. Frequent deployments introduce several challenges.

### Deployments on a Budget

Building and serving services with minimal resources is a significant challenge. One of the biggest issues I faced was long build times. When multiple services were pushed simultaneously, they had to wait in a queue for building and deployment. This created delays for QA, sometimes forcing them to wait up to 30 minutes before they could start testing, especially when the queue was long.

To mitigate this, I implemented caching and build artifact reuse. This optimization significantly reduced build times—from 5-8 minutes per service down to 3-5 minutes, making the pipeline much more efficient.

Another major challenge was limited disk space. The dedicated server provided for this project has only 100GB of disk space, which needs to accommodate Jenkins server, Five backend services, One frontend application, Other projects sharing the same server. With such constraints, I had to routinely clean up caches to prevent disk space from running out, as a full disk would cause build failures. To manage this, I set up a scheduler to automatically check and delete outdated caches when necessary. However, this approach has a trade-off—at times, when the cleanup runs, builds take longer because they cannot take advantage of caching, slightly slowing down the overall delivery process.

Regardless, I have done my best under these conditions. I realize there is still room for improvement, but given the limitations and the fact that the performance metrics are still acceptable, my options for further optimization are limited. I remain open to exploring new solutions, but in this situation, I have to accept the reality that compromising between efficiency and resource constraints is inevitable.

## Delivering Package

.NET applications rely on NuGet packages for dependency management and modular development. In this project, I used NuGet Sleet to create and manage a static NuGet feed.

To deliver these packages, I leveraged AWS S3 as the storage backend. The deployment process follows the steps outlined in  [https://github.com/emgarten/Sleet/blob/main/doc/feed-type-s3.md](https://github.com/emgarten/Sleet/blob/main/doc/feed-type-s3.md). However, by default, this configuration makes the feed publicly accessible, which poses a security risk.

To enhance security, we can implement a solution using AWS Lambda Function and AWS CloudFront. This involves configuring CloudFront to act as a proxy, collaborating with a Lambda function to validate authentication for incoming requests.

```javascript
'use strict';
exports.handler = (event, context, callback) => {

    // Get request and request headers
    const request = event.Records[0].cf.request;
    const headers = request.headers;

    // Configure authentication
    const authUser = 'USER';
    const authPass = 'PASSWORD';

    // Construct the Basic Auth string
    const authString = 'Basic ' + new Buffer(authUser + ':' + authPass).toString('base64');

    // Require Basic authentication
    if (typeof headers.authorization == 'undefined' || headers.authorization[0].value != authString) {
        const body = 'Unauthorized';
        const response = {
            status: '401',
            statusDescription: 'Unauthorized',
            body: body,
            headers: {
                'www-authenticate': [{key: 'WWW-Authenticate', value:'Basic'}]
            },
        };
        callback(null, response);
    }

     // Continue request processing if authentication passed
    callback(null, request);
};
```

While this approach improves security, it comes with certain limitations:

1. In order to collaborate with AWS CloudFront, the Lambda function must be deployed at the edge.
2. Due to the edge deployment, storing credentials in system variables is not feasible.

## What I Learned

Managing CI/CD with limited resources requires smart optimizations. Caching and build artifact reuse significantly reduced build times, but disk space limitations forced me to implement automated cache cleanup, which occasionally slowed delivery.

For NuGet package delivery, AWS S3 with NuGet Sleet was a lightweight solution, but securing it required CloudFront and Lambda.