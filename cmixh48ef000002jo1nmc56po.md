---
title: "Vertical Scaling vs Horizontal Scaling: What You Need to Know"
seoTitle: "Vertical vs Horizontal Scaling: Key Differences"
seoDescription: "Understand the differences between vertical and horizontal scaling with their key features, costs, complexities, and best use cases"
datePublished: Mon Dec 08 2025 18:17:06 GMT+0000 (Coordinated Universal Time)
cuid: cmixh48ef000002jo1nmc56po
slug: vertical-scaling-vs-horizontal-scaling-what-you-need-to-know
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1765188288168/ec83eae1-082a-4d93-aaa0-62887fed4845.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1765217793061/e9e4937a-831d-4fbd-bd86-575a6ac068d1.png
tags: aws, nginx, horizontal-scaling, vertical-scaling, ec2-instance, autoscaling-group

---

Well, you have come a long way in your DevOps journey. When your application gets more requests than usual, the server running your application can't handle the load. Traditionally, there are two ways to scale up your server, as you just saw in the banner.

* Vertical scaling - scale up
    
* Horizontal scaling - scale out
    

### Vertical scaling - scale up

So one of the possibilities is that if the server is getting more requests and it is getting overloaded, you could monitor and see that your CPU usage is going to 90%. So what you could do is think from first principles. If my CPU and storage are insufficient for my server, then one of the things we can do is get a bigger server with a more powerful CPU and storage, right?

Exactly, this is known as vertical scaling, where we just modify the server and get a bigger server that can handle the compute and complexities of your server. There are advantages to this method, such as being able to migrate to a bigger server without having to change the coding part.

Migration from a small server to a bigger server is not a big deal; you just have to deploy your application on the bigger compute machine. This machine could be any cloud provider compute, whether it may be **EC2 or a droplet,** right? But if you think from a cost perspective, then a bigger server equals more cost, right? Also, keep in mind that if your application grows even bigger, you will have to migrate to an even bigger machine. And if the question arises in your mind that at some point your server might not handle the load and fails, then your whole application will crash. In this case, you might think, why am I just using one bigger server, and why can't I run the same application on 5 small servers rather than one big server? This is where horizontal scaling comes in.

### Horizontal scalling - scale out

Well, instead of running the server on one bigger machine, we can run the same application on a bunch of small machines with the same configuration. In this setup, we need to figure out that every machine will have a different **IP address**, and your domain can only point to one machine at a time. This is where a new term comes in, called **Reverse Proxies**.

II will try to break down reverse proxies in simple terms. They are basically the gateway to your application. Think of it as your gate; it says that if you want to reach your application, I am the gate for it. Without me, you can't access it. (Here, we can access the application, but at a time, we can only point to a specific IP of the machine. We can't cater to every machine.) This tool allows you to fix this problem. It says that it will take all your requests coming from users and will transfer or proxy your request to one machine with a lower load, or it can follow some rule of OS scheduling called **Round Robin**. It basically tells you that the next request coming from your user will be forwarded to machine 1, then to machine 2, then to machine 3, one by one to each machine. This way, the load will be distributed.

### Artitecture of Reverse Proxies

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765217566243/573568a3-7533-40cf-b725-29d5665d4e1e.png align="center")

Again, this has some challenges and benefits. We had to optimize it as well as manage the complexity it will bring.

Below is the table I have provided; you can check it out.

Below is the Comparison Table in short

| Feature | Vertical Scaling | Horizontal Scaling |
| --- | --- | --- |
| Method | Increase hardware power | Add more machines |
| Cost | Expensive at extremes | Cheaper to scale |
| Failure | One machine failure = full outage | Redundancy, no total downtime |
| Complexity | Easy | More complex |
| Best For | Databases, old monolith apps | Web servers, microservices, cloud apps |

Please do like , comment and share

Thanks for reading :)