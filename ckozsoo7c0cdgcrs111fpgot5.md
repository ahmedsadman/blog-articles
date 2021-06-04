## Cloud deployment for absolute beginners

Let's say you have created an awesome web application that uses some Machine Learning to suggest movies you might like.  Now what? You have to share it with the world so that people can use it, right? This is where deployment comes into play. 

In this post, I will give a high-level idea of cloud deployment. You will be introduced to different terminologies and use cases. I will refrain from going into low-level technical details of deployment (like, how to host a Node.js web app). There are already a lot of articles available for that and I have linked them in this post.

Besides, there are a lot of posts explaining the deployment process. But rarely those articles give the whole idea of cloud deployment in a beginner-friendly way, thus this post. If you're new to cloud computing, you will find it worthy. After reading this, I hope many of your confusions will be cleared.

The basics you need to know:
- I assume you already know basic web development and you want to deploy your application
- Good command over Linux

Things that are discussed in this post:

- What is cloud deployment and how does it work?
- As a beginner, where should I start learning?
- Which cloud provider to use?
- What is the difference between Heroku and AWS
- What is localhost and private networks?
- NGINX/Apache? What are these?
- Networking essentials

...and many more


## What do you mean by *Cloud Deployment?*

Well, to deploy your application you can do the following:
1. Buy a server computer
2. Get a public IP to expose the server on the internet
3. Configure the OS to run your stack. For example, if your code is written in Python, install necessary packages (like Flask, Django etc) and configure them properly
4. Configure a web server to host your application to the internet
5. Keep your server running 24/7 at any cost. Because if your server is down, your application will be unreachable

**Seems scary, right?**


To add on top of that, please note that as your application grows, you will need more and more servers to handle the load. So, now you have to maintain multiple servers.

The above scenario is almost obsolete nowadays. Unless you're handling very sensitive data (like state secrets), you will be using a cloud provider to host your application on the internet.

So, basically, what a cloud provider does, solves points 1, 2, 3 (partially) and 5 for you. By just using some Web UI, you can spin up new computers for you in less than a few minutes. It will be automatically configured with an OS, will have a public IP and will never be down (well, almost never). This computer is residing in a remote place in a large data centre among many computers (like cloud particles in the sky), we can see it (read 'access it') but don't know exactly where it is located, just like the 'Cloud', thus the term 'Cloud Computing'.

How do they do it? They have enough money and manpower to handle millions of computer and resources at once. They serve you this service through some sophisticated mechanism.

There are currently many cloud providers. The leading ones are  [Amazon Web Services (AWS)](https://aws.amazon.com) ,  [Google Cloud Platform](https://cloud.google.com) ,  [Microsoft Azure](https://azure.microsoft.com/en-us) ,  [DigitalOcean](https://www.digitalocean.com)  etc. If you're already tinkering with deployments, you might have also heard of  [Heroku](https://www.heroku.com),  [Netlify ](https://www.netlify.com/) and  [Vercel](https://vercel.com/). There are very clear differences between these two type of providers.

## Heroku vs AWS
You can host your web application using both of these providers. But in Heroku, automation is the key. Heroku will automatically configure the OS and the tech stack for you. You just have to tell Heroku that you want to deploy a Node.js application and that's it! Thus, it will solve point 3 from above along with others. Also, it will automate the deployment process using GitHub, so that each time you push some code, your application will be automatically deployed. 

But, this is not what cloud deployment actually is. First of all, you will have little to no control over how your application is deployed and maintained. 

Secondly, these providers don't actually give you a computer/server, so you cannot remotely access those computers and do custom things on your own. They just provision you with some resource to run your web application and abstracts away from all the other things. 

Thirdly, such providers are much more costly than AWS or Google Cloud platform because of the extra automation. For example, if you want to run a production application in Heroku, you will have to pay a minimum of 5$ for each application. On the other hand, DigitalOcean will give you a server for 5$ where you can run 2-3 applications at once.

If you just want to host a hobby project, Heroku/Netlify is the best option. Because the basic usage tier for Heroku/Netlify is FREE. But if you really want to get into the world of deployments (DevOps), or even kickstart your Software Engineering career, you should get your hands dirty by using AWS, DigitalOcean or similar providers.

Remember, using cloud providers requires that you have some idea of [basic Linux commands](https://linuxize.com/post/basic-linux-commands). No, they won't give you nice little graphical interfaces to work with. The servers are mostly Linux based. You will have to run all your operations through the command line just like the good old days.

**Now the question, where should I start?** 

Well, pick a cloud provider, spin up a server and start tinkering with the command line. See how things work. Maybe follow a tutorial on how to configure your freshly picked server. But before going to that, let's decide on which cloud provider to use.


## Why DigitalOcean?
Contrary to popular opinions, I would advise starting with DigitalOcean.

**Pros:**
- It's much cheaper compared to other providers
- Has a clean and easy-to-use UI
- Has a lot of documentation written in a beginner-friendly way **(most important point)**

**Cons (don't worry about these now):**
- Has few services (for example, no support for automatic load balancing)
- Has small tier of computing resources, there's not a lot of options compared to other providers
- Some advanced networking and access management might not be possible or has to be done manually
- Availability is not as reliable as GCP or AWS.

If you're a beginner, there's a chance that the cons went over your head. At an entry-stage, those cons can be ignored.

All cloud providers will provide you with one thing, a computer system to work with. So, there's no learning difference at all. If you can work with DigitalOcean, you can work with AWS, Google Cloud Platform (GCP) or any other. In terms of learning, the only difference is their UI, access management and extra services they provide.

![DigitalOcean pricing comparison](https://cdn.hashnode.com/res/hashnode/image/upload/v1621681288185/JcWzSArIW.png)
*DigitalOcean pricing comparison for a 1Core - 2GB memory instance*

**How to use DigitalOcean?**

I'm not going to give you a step by step guide. If you're reading this, you should already know how to register for a service and add a credit card for payment.

You will be billed on an hourly basis. So if a specific computer costs 0.015$/hour and you keep that computer running for whole one month (24/7), it would cost you roughly 10$ at the end of the month.

In the DigitalOcean world, a single computing instance is called a *Droplet*. When you create a Droplet (a computer, or a computing instance), you can access that droplet and do whatever you want with it (like hosting web apps, playing with the terminal or even break the whole system apart 😉 )

Now how to create the droplet? Well, if there is an already well-written guide on the internet, there's no point of re-writing it, so follow this guide on  [how to create your first droplet
](https://docs.digitalocean.com/products/droplets/how-to/create/). Awesome documentation, right?

**What distribution/image to choose?**

We love Linux for servers. So I would recommend, from the Distributions tab, creating a plain Ubuntu machine with the cheapest plan (5$) for a start. This will give you a vanilla server to start with.

You could've also chosen some pre-built images from the Marketplace, then you wouldn't need to manually configure your server (discussed below). So for example, if you're hosting a WordPress website, you could have chosen that and the newly created server would come with Wordpress installed with all the necessary things configured.

*But, we are learners, we don't want to automate things for us, because there is no learning in that case. So, don't use Marketplace images for now.*

**What is SSH?**

Now that you have created your first remote computer, you have to access it using SSH (Secure Shell Protocol). In simple terms, SSH uses a cryptographic network protocol to securely connect you to a remote computer, from where you can access the computer through a terminal.

**Let's connect**

Now, let us see  [how to connect to your first droplet using SSH.](https://docs.digitalocean.com/products/droplets/how-to/connect-with-ssh/openssh/).

Well done! You're connected to your remote computer/server. Now it's time to [configure your server](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-18-04). Yes, a server needs some basic configuration to start with.

Okay, you have a server set up properly. Now, you have to install the necessary software to run your web application. For example, if you want to run a Python application, you have to install Python and the necessary packages. 

But till now, you have hosted your application in *localhost* using a development server. Now, you're on an actual remote server and want the whole world to see your app (or maybe not the world, just you, so that you can test), so there are some changes on how to deploy your application. 

I will provide you with some documentation of how to deploy your applications in DigitalOcean. I suggest digging into these articles only after completing this article of mine. Although these are written by DigitalOcean people, you can use these docs to deploy your apps to any server having any providers:

1.  [How To Set Up a Node.js Application for Production on Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-node-js-application-for-production-on-ubuntu-18-04) 
2.  [How To Serve Flask Applications with Gunicorn and Nginx on Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-serve-flask-applications-with-gunicorn-and-nginx-on-ubuntu-18-04)
3.  [How To Deploy a React Application with Nginx on Ubuntu 20.04](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-react-application-with-nginx-on-ubuntu-20-04) 

*Don't forget to praise DigitalOcean's nicely explained documentations.*

## Server networking 101
The above documentations seems nice, but as a learner, you have to be familiar with some basic server networking terminologies.

### localhost

Localhost, simply accessed by the URL `localhost` or `127.0.0.1`, is a hostname that refers to 'this' computer's network. By this, I mean the current computer I'm working with. It's useful to test different applications because the data is not sent over the internet, instead, all the data stays on the computer. In this case, your computer gets a simulated web server where you can load the necessary files of a program into the web servers and check its functionality.

Please note, the idea of referring to your own computer's virtual server while accessing `localhost`, is called the *loopback* mechanism.

*But, did you know, if you host an application in your localhost, people connected to the same network can easily access your application? How?*

In a private network (like connecting to a Wifi Router), your computer will have its own private IP address. As you're already on a network, other users should be able to access your computer if they know the IP address. By access, I mean several things. For example, users on the same network can `ssh` into your computer. Also, they can access hosted applications (like your web application).

What `localhost` or `127.0.0.1` basically does, it points to your own computer. For all OS, localhost is a standard to create a loopback address. 

Let's say, your private IP address is `192.168.30.5`. You're serving Node.js application on port 5000, so in your own computer, you would access it as `localhost:5000` or `127.0.0.1:5000`. Now, if another user on the same network wants to access your application, he has to visit `192.168.30.5:5000`. That's it. Beware, Your application is not available to the public internet, after all, you're in a *private network*.

To find your IP address, in Windows, type `ipconfig` in the command prompt. For Linux, generally, it is `ip addr show`. From the list, you have to find your Wifi Adapter and get the *IPv4* address that starts with `192.168.x.x`.


### ports
At the software level, a port is a logical construct that refers to a specific process running on the server. The ports starting from `0` to `1023` is reserved by the operating system. For own use cases, the common port range is `3000` and upwards. For a specific web application, the port is specified at the end of the IP address, using a colon:
```
127.0.0.1:5000
```

In the above example, the port number is `5000`. The server which is listening to `5000` will get the request. When the server is active (listening), port `5000` will be blocked and no other process can use it.

You can run multiple web applications in one machine by using different port address. Let's say you have a Node.js API running on PORT `5000`, Frontend running on PORT `5001` and database service is running on PORT `27017`. Assuming your droplet's IP address is `10.220.20.23`, you can access those as:

```
10.220.20.23:5000 // API
10.220.20.23:5001 // Frontend
10.220.20.23:27017 // database
```

### 127.0.0.1 vs 0.0.0.0

Based on previous discussions, you already know what `localhost` or `127.0.0.1` is. When deploying your application, you might also need to use the address `0.0.0.0` in your server config. In the context of servers, `0.0.0.0` means all IPv4 addresses on the local machine. If a host has two IP addresses, `192.168.1.1` and `10.1.2.1`, and a server running on the host listens on `0.0.0.0`, it will be reachable at both of those IPs.


### What are Nginx and Apache?
To process incoming connections from the outside world (the public internet), you need a web server.

-  A web server knows how to talk to the internet using HTTP and HTTPS network protocols
- It can route requests to different services based on the request type (fancy name is *Reverse Proxy*). You might have the backend API and frontend hosted on the same machine. Nginx/Apache can redirect API/backend requests to API service and frontend requests to frontend service
- Helps you set up domain names
- Creating different rules for different URLs. For example, you might want to redirect users from 'oldsite.com' to 'newsite.com'.
- Caching
- SSL

...and a lot of things.

Things that a web server like Nginx can do is outside the scope of this article. The above listed are the very basic things.

A web server is essential to run your web application. In a regular setup, you will always use nginx/apache to serve your application. Even if in some cases you're not using them directly, do know that it is being used in the top levels somewhere.

**Then what are Gunicorn and uWSGI in the Python world?**

Gunicorn or uWSGI are * [application servers](https://en.wikipedia.org/wiki/Application_server) *. To talk to Python application we need  [WSGI](https://en.wikipedia.org/wiki/Web_Server_Gateway_Interface).  Gunicorn and Nginx have WSGI implemented, where Nginx/Apache doesn't. Nginx/Apache is used for more general-purpose server works. Gunicorn or uWSGI works as a middleman between Nginx and the Python web application. 

So, in a Python set-up, Nginx will be responsible for reverse proxying, caching, SSL, serving static files etc. While dynamic requests that should be handled by the Flask/Django application will be passed to Gunicorn/uWSGI. The interaction between these two are as follows:

1. nginx receives the incoming request from the internet
2. Based on the Nginx config it can decide if this request needs to be passed to Gunicorn/uWSGI
3. Gunicorn passes the request to the Python application
4. The application sends back the result to Gunicorn after processing
5. Gunicorn sends back the result to Nginx
6. nginx replies to the user with the response

**What if my user increases and I have to handle 1000 req/sec? The current deployment process is too lengthy and counter-productive, how do I automate it...?**

Woah! Slow down. We just scratched the surface here. Cloud deployment is a whole different topic on which you can do full-year courses. Nowadays, the sector is called DevOps and people working on DevOps are called DevOps Engineers. Also, Site Reliability Engineers (similar to DevOps but not identical). Once you're comfortable with the basics, you can do courses on DevOps and learn more. An interesting world awaits you.


## Where to go from here?
Once you are confident that you understand the basics of server deployment, try these:
- Learn shell scripting. It's the most underrated tool for an Engineer working in the technical or IT sector. Whenever you want to automate something or add custom behavior you have to do shell scripting in Linux. Start learning from a  [crash course here.](https://dev.to/godcrampy/the-missing-shell-scripting-crash-course-37mk) 
- Learn Docker. It's the most used deployment tool no matter where you go. For this, you can follow  [Docker Mastery course in Udemy.](https://www.udemy.com/course/docker-mastery/) 
- Learn Jenkins. You can use  [CI/CD Jenkins course from Udemy.](https://www.udemy.com/course/learn-devops-ci-cd-with-jenkins-using-pipelines-and-docker/)
- If you're really serious about DevOps and want to pursue your career in this role, you should also learn  [Kubernetes](https://www.udemy.com/course/learn-devops-the-complete-kubernetes-course/)  

That's it for today, folks. I will try to write more awesome articles for both beginners and advanced level users in the future. I will also share my technical experiences here. Stay tuned. If you like this article, please share it with your network. Thank you for reading this to the end.