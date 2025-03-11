# Automated WhatsApp Service

## Background

Every company strives to build an employee information system that covers attendance and more. Employees can work from anywhere, whether at a client’s site or in the office. However, they often forget to mark their attendance, making a reminder system necessary.

Most company attendance systems are web-based, which means push notifications are not an option. After exploring various solutions, the most effective method was determined to be WhatsApp, as 100% of employees use it.

## **The Process I Worked On**

Sending WhatsApp messages is easy, but what if the sender is a computer? WhatsApp provides an official API, but it’s too expensive, so I had to find an alternative. Thanks to Baileys, a library that allows interaction with WhatsApp, I found a way. It works like a web scraper, fetching and setting data while simulating WhatsApp Web.

However, there was a challenge—I couldn’t send messages directly through Baileys. To solve this, I had to create a wrapper to interact with it. Fortunately, I had already built one before 😄, so I can say this project was 90% done before it even started. I published the API on GitHub: [github.com/daimus/whatsapp-api](https://github.com/daimus/whatsapp-api). This project allows interaction with WhatsApp via REST API or a scheduler.

So, what was the remaining 10%? Implementing the reminder was straightforward—I just needed a template and a list of employee phone numbers. However, there was an additional requirement: if an employee hadn’t marked attendance by 3 PM or 4 PM, they should receive another reminder. To handle this, I had to create a job that gathers a list of employees with incomplete attendance and sends them a reminder.

At first, everything seemed fine, but then I realized reminders shouldn’t be sent on holidays. To fix this, I created a list of public holidays in Indonesia and excluded those dates from the job execution.

So far, the reminder system works well—the scheduler, reminders, and job exclusions all function as expected. Since it proved useful, I decided to improve it by adding a web UI for Human Capital (HC). This was well received, as HC could also use it to send birthday greetings and broadcast important company announcements. Even after I left the company, this reminder system continued to be used for attendance reminders and message broadcasting.