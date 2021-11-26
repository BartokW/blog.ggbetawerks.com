---
title: The Wayback Machine Saved Me Days of Work
date: '2021-05-18 14:06:00'
header: 
  image: "/assets/images/waybackmachine.svg"
  teaser: "/assets/images/waybackmachine.svg"
tags:
- software-development
---

Over the years I've been helping to maintain an old website written using ASP, not ASP.net, but classic ASP using VB Script. This website has a small online store for a few products and several online courses that they run. &nbsp;We were using a payment processor that allowed redirecting, meaning when we needed to process a payment we would redirect to the payment processor's website passing in name, address, and amount to charge, and once it was processed it would redirect back to our site with the confirmation code, and other information about proof of payment.

Then one day I get an email saying that the payment processor is making several changes, including stopping support for older encryption methods (TLS) and we would need to make changes to handle server to server api calls for processing the payments. This method is all done using JSON (JavaScript Object Notation) for both the request and response. &nbsp;And of course I didn't learn about it until it was less than a month until the switchover.

A quick search revealed there's no built-in support for JSON, so my search for a way to deal with JSON in ASP began. All the recommendations were for one particular library located at ( [https://www.aspjson.com/](https://www.aspjson.com/) ) which did not appear to exist any more. &nbsp;I started looking at options, and figured I could easily do a formatted string to build the JSON for the request, &nbsp;but the response would mean needing to parse JSON and that seemed like a ton of work.

Since I knew that website had been online at one point I decided to try the Wayback Machine ( [https://web.archive.org/](https://web.archive.org/) ) and see if I could get anything about the library there. It turned out that the whole site was indexed, and I was able to click on the download link and it actually provided me with the file. &nbsp;I quickly implemented the changes needed on the old ASP website and got everything running complete with using JSON.

Note: When I decided to write out this story, I went looking to see what library it was again, and it turns out that the website is back ( [https://www.aspjson.com/](https://www.aspjson.com/) ), so it may have just been down for the weekend when I needed it, but either way the Wayback Machine saved me.

