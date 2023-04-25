---
title: "Intro to N8N"
author: "Justin Brant"
# cover: "/img/cover.jpg"
tags: ["VMUG"]
date: 2023-04-19T14:19:13-04:00
draft: false
---

## Back in the saddle

On April 19th I gave a presentation to Fort Wayne VMUG around N8N and my current escapades down that rabbit hole of a automation framework. I have a devops mindset but wasn't sure where I was going with this application other than some specifc use cases which I will discuss later in this article. Lets jump into the "What is it?". 

## What is it?

n8n is an open-source workflow automation tool that allows users to connect different apps and services together to automate tasks and processes without any coding. It provides a user-friendly interface to create workflows by dragging and dropping nodes, which represent different actions, and connecting them to build a workflow. n8n supports various integrations with popular apps and services like Google Sheets, Slack, Trello, and many more, making it a powerful tool for automating business processes and workflows.
- Source ChatGPT

## What do I really use it for? 
My specific use cases were essentially what I will call API Translation. We use a program in our infrastructure that sends alerts via webhook. This is great we can integrate this into our chat system! Yay no email white noise from this. Let work through that and see what I can break along the way.

## Webhook JSON 
We configure our webhook and our chat system wants a specfic layout when it comes to that request. We lay out the request to look something like this. 

``` json
{
  "roomId":"Y2lzY29zcGFyazovL3VzL1JPT00vYmJjZWIxYWQtNDNmMS0zYjU4LTkxNDctZjE0YmIwYzRkMTU0",
  "text": "Some Message"
}
```

Everything looks like normal JSON and using postman to test this works swimmingly. 

## Wait.... Wheres the content-type?
Neat found our first bug going from this applicaiton to our chat system. It gives some generic message about missing data but no real reason why. After some digging using the site https://webhook.site/ which is super neato and you should give it a look. I determined that when we put our custom JSON in vs leaving the custom field blank it rips the `content-type = application/json` out of the header. Well this is awkward. If we stick with basic no formatting the content won't make it to chat and if we get it in the right format then its missing content-type with no way to add it back into the header. This is where N8N comes in. 

## Lets setup N8N to fix this

`docker run -it --rm --name n8n -p 5678:5678 -v ~/.n8n:/home/node/.n8n docker.n8n.io/n8nio/n8n`

{{< figure src="/img/easy-button.jpeg" width=95% layout="responsive" >}}






