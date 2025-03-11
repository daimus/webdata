# Zonation Based Enrollment System

## Background: Adapting to New Educational Policies

With the Ministry of Education implementing a zoning-based student admission policy, schools must prioritize applicants based on geographical proximity. This policy ensures that students enroll in schools closest to their residence, reducing travel burdens and promoting equal access to education.

## The Challenges: Avoid State Bankruptcy

In a zoning system, applicants provide their home address during registration. The system then calculates the distance from their residence to the school and ranks applicants based on proximity. Schools with limited seats admit students who live closest first. To automate this process, many institutions rely on mapping services like Google Maps API to compute distances accurately.

Traditionally, we used this process to determine applicants:

1. **Geocoding Addresses** – Converting home addresses into latitude and longitude coordinates.
2. **Calculating Route Distance** – The API computes the actual travel distance or time between the applicant’s location and the school, considering real-world road networks.
3. **Sorting Applicants by Distance** – The system ranks students based on computed distances and report card grades.

The first process absolutely must use a third-party map service because users need visuals such as street names, cardinal directions, landmarks, area names and so on to put marker of their home. But in the second process we has significant drawbacks:

1. **High API Costs** – Google charges per request, and with thousands of applicants, costs escalate rapidly.
2. **Rate Limits** – API request limits can slow down or disrupt the admission process.
3. **Reliance on Internet Connectivity** – Every query requires a live API call, making the system dependent on stable internet access.

At this moment I realized that coordinates are numbers which means they can be calculated. Thanks to Haversine formula, we implement a more efficient distance calculation locally instead of querying an external API for each applicant.

The Haversine formula calculates the shortest distance between two points on a sphere using their coordinates:

```jsx
d = 2r * arcsin(√(sin²(Δϕ/2) + cos(ϕ₁) * cos(ϕ₂) * sin²(Δλ/2)))
```

Where:

- *d*: Distance between the two points.
- r*r*: Earth’s radius (≈ 6,371 km).
- ϕ1,ϕ2*ϕ*1​,*ϕ*2​: Latitude of point 1 and point 2 in radians.
- λ1,λ2*λ*1​,*λ*2​: Longitude of point 1 and point 2 in radians.
- Δϕ=ϕ2−ϕ1Δ*ϕ*=*ϕ*2​−*ϕ*1​: Difference in latitude.
- Δλ=λ2−λ1Δ*λ*=*λ*2​−*λ*1​: Difference in longitude.

With this formula we only need to store the coordinates of the school and applicant. We can even implement the formula into a SQL view so we can find out the distance in real time.

```jsx
SELECT 
    2 * 6371 * ASIN(
        SQRT(
            SIN(RADIANS((lat2 - lat1) / 2)) * SIN(RADIANS((lat2 - lat1) / 2)) + 
            COS(RADIANS(lat1)) * COS(RADIANS(lat2)) * 
            SIN(RADIANS((lon2 - lon1) / 2)) * SIN(RADIANS((lon2 - lon1) / 2))
        )
    ) AS distance
FROM 
    applicants;
```

### Why Haversine Works Better

- **Zero API Costs** – Eliminates reliance on Google Maps, significantly reducing expenses.
- **Fast and Scalable** – Runs efficiently on local servers, handling high volumes of applicants without delays.
- **Offline Processing** – Does not require internet access, ensuring reliability even in network-limited environments.

By integrating Haversine calculations, we optimized student admission processing while maintaining accuracy and cost efficiency.

## The Challenges: Overcoming Enrollment Roadblocks

When the enrollment period opens, a large number of applicants attempt to register at the same time. This surge can overwhelm the system, leading to performance issues such as slow response times or even downtime. Additionally, since registration slots are limited based on school quotas, there is a risk of overbooking if multiple users submit their applications simultaneously.

To ensure fairness and system stability, we implemented a registration queue system with database locking:

1. Sequential Registration Numbers – Each applicant is assigned a unique registration number in the order their request is processed, ensuring a first-come, first-served basis.
2. Optimistic & Pessimistic Locking – To prevent race conditions where multiple applicants attempt to claim the same slot, we implemented database locking strategies:
    - Optimistic Locking: The system checks if the available slots have changed before finalizing a transaction. If a conflict is detected, the user must retry.
    - Pessimistic Locking: The system temporarily locks a slot while a transaction is in progress, ensuring no other applicant can claim it simultaneously.
3. Job Queue for Processing – Instead of processing all registrations synchronously, incoming requests are placed in a queue and handled in sequence, ensuring stable performance even under high load.

### The Benefits

- Prevents Overbooking – Ensures that accepted applicants do not exceed the available school quota.
- Maintains System Stability – Prevents crashes due to high traffic spikes.
- Fair Enrollment Process – Guarantees that registrations are processed in the correct order without unfair advantages.

By implementing these strategies, the system efficiently manages high registration traffic while ensuring accurate and fair admission processing.

## Highlight My Role

As the Full-Stack Developer in this project, my responsibilities included:

- Built backend system including user management, role & policy management, school management, applicants registration, sorting candidate, notification system.
- Implemented database locking to prevent overbooking.
- Optimized performance to manage high traffic loads.
- Implementing frontend design, integrate with backend.
- Conducted functional testing and load testing.

By managing both backend and frontend development while ensuring rigorous testing, I contributed to building a robust and efficient online enrollment system.

## Key Learning

Developing a large-scale online enrollment system requires careful consideration of performance, cost efficiency, and data integrity. By replacing high-cost distance calculations with the Haversine formula, we significantly reduced reliance on third-party APIs while maintaining accuracy. Additionally, managing high traffic surges with database locking and queuing mechanisms ensured system stability and prevented overbooking. These strategies highlight the importance of designing scalable, optimized architectures that balance efficiency, fairness, and robustness in critical digital services.