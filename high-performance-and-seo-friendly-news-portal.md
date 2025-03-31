# Hight Performance and SEO Friendly News Portal

## Project Overview

The Online News Portal is a platform for delivering news to readers. It features a clean and user-friendly design for a comfortable reading experience and easy navigation. The system supports multiple authors, an editorial workflow, content recommendations, ads management, user interactions like reactions and comments, and is optimized for high performance and SEO.

With these features in place, developing the platform comes with its own challenges, from SEO optimization to managing billion request to ensuring high performance. The next section explores these challenges and how they are addressed.

## SEO

As the saying goes, “The best place to hide is the second page of Google search,” making SEO a crucial aspect of this project. I implement best practices, ensuring proper meta tags, semantic HTML, and an SEO-friendly page structure.

Page performance is also a priority. I minimize asset sizes, optimize loading speed, and even built a function to automatically reduce image sizes for better performance. Beyond on-page SEO, I apply off-page strategies like backlink partnerships.

User experience is equally important. I ensure the layout adapts well across desktops, tablets, and smartphones, carefully positioning headings, text, and buttons. Even shared link previews are optimized for better visibility.

These efforts have yielded solid results, with good scores on Google Lighthouse and other SEO tools. Additionally, the website has gained a decent number of visitors for a regional news portal.

## Manage Traffic

Serving news pages isn’t resource-intensive, but to ensure fast delivery, I implemented techniques like database indexing, caching, and CDN caching. With the current server setup, the system can handle over thousand requests per second, meaning it’s capable of serving million of users daily. Unfortunately, we don’t have much actual user, we only have thousand visits per day based on cloudflare’s report.

Beyond handling real users, online platforms are vulnerable to cyberattacks like DDoS. From the start, I integrated Cloudflare and an internal firewall for protection. This proved essential when the site faced a DDoS attack with billions of requests. Thanks to the security layers, real users remained unaffected.

Links:
- [https://suarajatimpost.com](https://suarajatimpost.com)
- [https://beritacrypto.com](https://beritacrypto.com)