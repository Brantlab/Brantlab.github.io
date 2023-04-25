---
title: "Intro to N8N"
author: "Justin Brant"
# cover: "/img/cover.jpg"
tags: ["VMUG", "Automation"]
date: 2023-04-25T14:19:13-04:00
draft: false
---

## Back in the saddle

On April 19th I gave a presentation to Fort Wayne VMUG around N8N and my current escapades down that rabbit hole of a automation framework. I have a devops mindset but wasn't sure where I was going with this application other than some specific use cases which I will discuss later in this article. Lets jump into the "What is it?".  

## What is it?

>n8n is an open-source workflow automation tool that allows users to connect different apps and services together to automate tasks and processes without any coding. It provides a user-friendly interface to create workflows by dragging and dropping nodes, which represent different actions, and connecting them to build a workflow. n8n supports various integrations with popular apps and services like Google Sheets, Slack, Trello, and many more, making it a powerful tool for automating business processes and workflows.

Source ChatGPT

## What do I really use it for? 
My specific use cases were essentially what I will call API Translation. We use a program in our infrastructure that sends alerts via webhook. This is great we can integrate this into our chat system! Yay no email white noise from this. Let work through that and see what I can break along the way.

## Webhook JSON 
We configure our webhook and our chat system wants a specific layout when it comes to that request. We lay out the request to look something like this.  

``` json
{
  "roomId":"Y2lzY29zcGFyazovL3VzL1JPT00vYmJjZWIxYWQtNDNmMS0zYjU4LTkxNDctZjE0YmIwYzRkMTU0",
  "text": "Some Message"
}
```

Everything looks like normal JSON and using postman to test this works swimmingly. 

## Wait.... Wheres the content-type?
Neat found our first bug going from this application to our chat system. It gives some generic message about missing data but no real reason why. After some digging using the site https://webhook.site/ which is super neato and you should give it a look. I determined that when we put our custom JSON in vs leaving the custom field blank it rips the `content-type = application/json` out of the header. Well this is awkward. If we stick with basic no formatting the content won't make it to chat and if we get it in the right format then its missing content-type with no way to add it back into the header. This is where N8N comes in.  

## Lets setup N8N to fix this

`docker run -it --rm --name n8n -p 5678:5678 -v ~/.n8n:/home/node/.n8n docker.n8n.io/n8nio/n8n`

{{< figure src="/img/easy-button.jpeg" width=25% layout="responsive" >}}

Seriously there is more to this but this stands up the service then you can go along to http://whateverDockerIPis.com:5678 and it will walk you through the setup. It is pretty simple to setup account and you will be good to go.  

## Ok so what now?

Once logged in we are going to create our first workflow. This is really simple as well. You should be noticing this should simplify life and not make it harder.  

{{< figure src="/img/N8N/createwf.gif" width=50% layout="responsive" >}}

Proceed with creating your first node by hitting the plus and select webhook call. 

{{< figure src="/img/N8N/newnode.gif" width=50% layout="responsive" >}}

Once that is created we have the options as listed in the screenshot. Lets work through that list.  

1. Test URL. Test allows us to step through our automation one step at a time to manipulate the data in a way that we want to and see how the data get affected as it transgresses the work flow.

2. Production URL. Well it's production. It runs through the workflow without stopping and is only effective when you save the workflow.  

3. Authentication allows for you to configure authentication on your incoming webhook which can be nice to prevent others who may stumble across your endpoint with not so great intentions.  

4. Credential for Header Auth. This allows you to pick the authentication you want to use. While not best practice you could reuse the auth header across multiple endpoints.  

5. We are going to skip the ones you should know using APIs

6. Path allows you to have a custom hook like https://webhook.example.com/ChatGPT vs using some https://webhook.example.com/GUID-Not-easily-remembered

## Let get our first test data

{{< figure src="/img/N8N/firstdata.png" width=75% layout="responsive" >}}

Ok so this is scrubbed because #Production but for our use case this is able to be used for this article. 

If we parse this data your going to say "Justin I thought you said your incoming webhook ripped the content-type out?" Yes I did say that when you do a custom webhook format but this is just generic since I have the ability to format all my code I didn't feel the need to format the data anymore at the source. 

## 2nd Node

I added a switch just as this is currently a growing project and wanted to have the ability to "filter" to a different step in the work flow. 

{{< figure src="/img/N8N/layout.png" width=50% layout="responsive" >}}
{{< figure src="/img/N8N/switchdata.png" width=75% layout="responsive" >}}

With this data into this 2nd node I am filtering locked account messages to go to output 1 and the rest or default to go out output 0 with no data modifications. 

## 3rd Node

Below is what I did with our data in the 3rd node. I moved the data around to fit what we wanted to deliver to our new webhook request into our chat system. This is a simple as dragging our various fields into our variable fields and they show up as `{{ $json.body.description }}` for example. 

{{< figure src="/img/N8N/3rdnode(1).png" layout="responsive" >}}
{{< figure src="/img/N8N/3rdnode(2).png" layout="responsive" >}}

### This is the JSON in my variable field

``` json
{
  "roomId": "The room ID", 
  "markdown" : "Alert: {{ $json.body.description }} \n IP: {{ $json.body.remote_address }} \n Category: {{ $json.body.category }}"
}
```

## The ultimate request

``` json
[
  {
    "id":"The ID",
    "roomId":"The Room ID",
    "roomType":"group",
    "text":"Alert: Device Web Vault Edge is approved for user $User IP: Category: security",
    "personId":"Person ID",
    "personEmail":"something@email.com",
    "markdown":"Alert: Device Web Vault Edge is approved for user IP: Category: security",
    "html":"<p>Alert: Device Web Vault Edge is approved for user <br>IP:<br>Category: security</p>",
    "created":"2023-04-22T16:10:56.056Z"
  }
]
```

### Profit!

{{< figure src="/img/N8N/results.png" layout="responsive" >}}


This has been an amazing rabbit hole and as I do more with it I will add to my blog the different things. This isn't even scratching the surface as there are thousands of use cases. 

Check them out here https://n8n.io/