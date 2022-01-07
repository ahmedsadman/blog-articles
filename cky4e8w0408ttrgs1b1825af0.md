## Introduction to Microservices architecture with Tinder

Howdy folks? It's been a long time since I wrote an article. Today, I'm restarting with this which will aid beginners to learn what Microservice architecture is, how it works and its pros and cons. After that,  we will design a mock Tinder application using microservices.


Microservices is an architectural design pattern that is followed by many tech giants to create highly scalable and reliable systems. Netflix is the first company that popularized the use of microservices architecture. Before learning about Microservices, we need to understand what was there before Microservice came in.

# Big Old Monoliths
To create a basic web application, we need *a) server* and *b) database* . These pieces together make a single *service*. If your application grows in size and gains popularity, it will be not so basic after a few months. Soon you will be going through thousands (if not millions) of lines of code and countless files and folders all working together to provide the functionality to the customers. It might be hosted in version control like Github and all of your teammates will be working on that single repository. This is a monolith architecture. All kinds of functionality for your application like user auth, user profile and core functionalities reside in a single place.

## Pros
1. Easy to detect issues throughout the system
2. Infrastructure maintenance is relatively simple and less expensive
3. Easy to onboard new team members as the codebase is in a single place
4. Doesn't require a sophisticated engineering team to handle the system

## Cons
1. Difficult to scale properly as each and every functionality is tightly coupled
2. Adding new features and code maintenance can get overwhelming very easily
3. No fault isolation. If one part of your application goes down, the whole application goes down.
4. Difficult to distribute development across teams
5. Usually limited to a single type of tech stack. So if you're working with the MERN stack, you will have to deliver the whole functionality in that single stack.
6. Build and deploy process can be very slow.



# Let's split them up, onto Microservices
When we want to adopt Microservices, we need to do separation of concerns. Instead of creating a giant single service (monoliths), we separate them based on their functionality and create small services (microservices). There are some certain criterias like data duplication, scope, independence etc while doing this which will make it an actual microservice, but we can skip those part for this article. All of those services might have their own databases and own servers. For example, if you're creating a social media application like Facebook, then it might have the following services:

1. Authentication Service: Responsible for user authentication and authorization
2. File storage service: Responsible for serving and storing media files
3. Friends service: Store friend relations
4. Feed Service: Generate your personal feeds based on relevancy

...and many more. Please note, if you used a monolith architecture, all of these services would be treated as a single one.

## Pros

1. Each unit is decoupled so upgrading and maintaining a single part is much easier.
2. Scaling is more flexible and easier. Each microservice can be scaled independently. You can quickly point out which service is load-intensive and scale just that.
3. Improved fault isolation. So if your 'file storage' service goes down your 'authentication' and 'friends' service would still work. This is possible because each service has its own set of machines (servers, databases etc).
4. Easy to distribute development to different teams. One team can be responsible for one microservice
5. You can use different tech stacks for different microservices. So the service where machine-learning is involved, you can use Python. For the service which is IO heavy, you can use something like Node.js and so on. You get the idea.
6. It can help to board new members easily assuming one team looks after a single service.
7. Build and deploy will be extremely quick as you're working on a small service.

## Cons
1. The system architecture or infrastructure gets extremely complex
2. Needs a very capable and much bigger engineering team
3. Expensive
4. Detecting issues or debugging gets relatively difficult as different services are communicating over the network. More tools are needed for logging and monitoring
5. Inability to properly draw boundaries among different microservices would cause more trouble than solving them.



> 
WARNING: The concepts here are very simplified for new learners, so this article should not be treated as a full-fledged guide to microservices. In reality, separating concerns is not as easy as it seems. They are called 'micro' for a reason (not 'mini'). Don't confuse 'microservices' with 'service-oriented architecture'. Yes, they are different!


 
**Monoliths or Microservices, which one is better?**  
Depends. Unless you're working on an application that is complex in nature and you need to scale to millions of active users, you can safely avoid microservices. Even then, a monolith might be a better option depending on your team and capability. Also, it is not possible to get a full practical hands-on experience on Microservices unless you're working in a professional setting.


# Designing Tinder using Microservice architecture
Now comes the fun part, we will design a mock version of the popular dating application Tinder. Please note, this is not an actual design of Tinder and is for educational purposes only.

Before jumping to the microservice-based architecture, let's see how it would look if it's built using the monolith architecture:


![tinder-monolith.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1641543670594/SYxTy8FkW.png)



Let's jump into the microservices design part. For Tinder, I've decided to create the following microservices:

1. Gateway service: This is the service where the users will connect from the public internet. It would be responsible to process some initial data and directing the request to the relevant services as required. Typically, every kind of microservice-based application should have a gateway service like this.

2. Auth Service: Used to authenticate/authorize user requests

3. Storage Service: Will be used to storing and serve all kinds of media files from the user, such as images, videos etc

4. Messaging Service: A socket-based messaging server that will be used for one-on-one chatting/messaging purposes among the matched users

5. Profile Service: Here we will be storing users and their matchings related information

This is enough for our learning. In reality, considering that Tinder uses microservices, it is definitely not only 5 services. It might be 50 or 100 or 150, we may never know. Just as an example, Netflix uses 300+ microservices to keep the platform running smoothly.

Using this architecture, let's see how this boils down into a simple diagram


![tinder-microservice-1.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1641545701296/CbD0YtLnA.png)

Each circle represents a running container, usually, it's a docker container. A container might run on the same or a different machine. Some containers together form a cluster. The cluster decides how to balance the loads and communicate among different containers. Things like this are handled with a container orchestration tool, such as Kubernetes.


Let's talk about scaling a little bit. Let's say, after some logging and monitoring the engineers at Tinder see that the most demanding service is the messaging service (obviously!), so they can just scale the messaging service like this:


![tinder-microservice-scaled.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1641549534678/PSf29aXQT.png)

We just horizontally scaled the messaging containers. Please note, some earlier portions of the image is cut off to fit smaller screens. The rest of the architecture is the same as the previous images.

## Handling the communication chaos

Now, it is very likely that when some event occurs, multiple services have to know about it. *User registration* is such a scenario. When the user registration happens, we need to do the following:

1. Store the user-related information in the Auth service and send processed data in Profile and Messaging service
2. Store and process the user media (like profile pictures, gallery images) in the Storage service
3. Store the user details process data in the Profile service
4. Store and process session related information in the messaging service.

We can make the communication direct among different services. But, this is not the perfect solution. It will easily get tangled and complex as the communication is happening in many places. The left side of the figure below depicts this situation. Imagine what would happen when you have to communicate across 100 services. Also, from an engineering perspective, we're tightly coupling the services as soon as we put inter-communication logic inside them. The goal should be the exact opposite, which is *decoupling*. By decoupling, we can ensure maximum scalability.


![com-chaos.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1641550426918/dQjRpj6UI.png)


To solve this issue, microservices use a common bus channel for communication, this is a simple message queue where there are publishers (or producers) and subscribers (or consumers). Any service can subscribe and listen to a message queue. One service can be both consumer and producer at the same time. The right side of the figure depicts this.
 
This is much more simple. No service is directly communicating with each other. There is a common channel where everyone is publishing the events with necessary data. As a result, one service doesn't have to know about another service, which ensures decoupling. Those services subscribed will be informed of all the data in the channel. 


## Technologies used for microservices

For deployment, we would need a combination of Docker, Kubernetes, Helm, Ansible, Jenkins and a few other tools. If you go to the microservice route, you obviously need a very capable DevOps team.

For inter-service communication, as I already described earlier - RabbitMQ, Amazon SQS, Kinesis etc are used.

Logging is an important part. To implement proper logging and monitoring we need a combination of Elasticsearch, Logstash, Kibana, Newrelic, Sentry etc.

That's all I know. There are definitely more tools that I'm simply not aware of.


## Conclusion
As a beginner step, this article should be enough. If you want to explore more in-depth knowledge on microservices, you can watch this amazing  [guide on Microservices from the one and only Netflix.](https://www.youtube.com/watch?v=CZ3wIuvmHeM). That's all for today. Please leave your feedback or questions in the comments below. Have a good day.