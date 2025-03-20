# Gas Station Attendance & Tank Meter Tracker

This project was initiated to address two major issues: the disorganized attendance records and frequent delays in predicting fuel pump tank refills.

The idea of digitization immediately came to mind to enable faster data synchronization. However, small businesses (SMEs) often lack the necessary resources. They cannot afford to invest in digitalization without a clear impact, and building a fully developed system from scratch would require too much time and effort. Even third-party applications were not a good fit.

Fortunately, with the rise of no-code tools, it’s now possible to create applications quickly. This allows us to develop a prototype first before investing in a production-ready system. Clients also find it easier to understand when they can see a visual representation of the solution.

## How This App Works?

This application simply records employee check-in and check-out data. Each attendance entry includes the timestamp and tank meter reading. With this data, we can solve the two key issues: tracking employee work hours and maintaining updated tank volume records.

But it doesn’t stop there. To prevent entry errors, the attendance feature needs to be more advanced. That’s why I added:

- Easy of access: can be installed on mobile device (PWA)
- Notification: for attendance reminder
- Location detection.
- Image metadata validation.

## Day 1 Development: No-Code

Since this project has a relatively simple business flow, I thought—why use complex and fancy technology when no-code can do the job? That’s why I quickly set up Directus and started exploring how it works.

Developing with Directus is slightly different from conventional development. Normally, I normalize data up to the third normal form (3NF), but Directus has query limitations. So, I relaxed the normalization and focused on how the data could be visualized effectively.

The next challenge was managing data entry. Some data couldn’t be inserted directly; it required validation and calculations before being stored in the database. That’s where Directus Flows came in. Building these flows required multiple iterations—I had to understand what data was being processed and what the expected output was. Additionally, flows in Directus are limited to built-in plugins, and executed code runs in isolation. Even using external libraries requires wrapping them into a custom plugin.

Fortunately, the business logic in this project is straightforward. Although implementing check-in functionality required 3–4 flows with multiple steps, I managed to get it working.

Another key feature that drew me to Directus was its dashboarding capabilities. Since I had already designed the data structure with dashboard visualization in mind, integrating it into Directus was smooth. The platform provided a comprehensive set of components, making it easy to bring my vision to life.

![backoffice1](https://raw.githubusercontent.com/daimus/webdata/main/images/pumphub-bo1.png)
![backoffice2](https://raw.githubusercontent.com/daimus/webdata/main/images/pumphub-bo2.png)

## Day 2 Development: Attendance Frontend

How should I build the attendance frontend? Giving employees direct access to Directus wasn’t an option—it’s not user-friendly. So, I decided to create a dedicated frontend for employee check-ins. The challenge was to develop it as quickly as I had built the backend and back office.

I chose Next.js, a framework I’m familiar with, because it provides a ready-to-use PWA template. For the design, I relied on ShadCN, which offers pre-built UI blocks that streamlined the process.

With the design in place, the next step was backend integration. Fortunately, Directus provides a JavaScript SDK, allowing me to connect the app with minimal tweaks.

![fe](https://raw.githubusercontent.com/daimus/webdata/main/images/pumphub-f2.PNG)

## Completed in 2 Days

Yes, in just two days, the application was up and running. While not all planned features were fully implemented, the core functionality was ready. This rapid development allowed me to quickly test the system and demonstrate it to the client.

Now, with a working prototype, the next step is improving and expanding the features based on client needs. This agile process ensures that every enhancement adds real value to the final product.