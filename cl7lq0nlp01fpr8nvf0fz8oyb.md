## Docker: A conceptual overview

In modern DevOps, Docker is considered the most popular set of services for application containerization. In this article, we will NOT be covering a Docker tutorial, because a lot of good tutorials are already available on the internet. Instead, we will dive into the theory of how Docker works behind the scenes.

"Docker" is not a single tool, but rather a set of tools/components:
1. Docker Client (user CLI to talk with other Docker components)
2. Docker Engine (the core engine with all the magic behind virtualization)
3. Docker Compose (run multiple containers)
4. Volumes (persistent disk space)
5. Docker Hub (place to store built images) etc.


## What is an Image?
A single file with all the program dependencies and configuration included, which can be used to create containers that can be run anywhere. The *image* works as the **blueprint** to create *containers*.

## What is a Container?
An instance of an image which runs a program. This container can be run in any computer system without any extra hassle


Docker is used to create images and also run containers derived from those images. Images can be stored in **Container Registries** such as **Docker Hub**. You can think of Docker Hub as GitHub, but for Docker Images. Please note, you can also create your private container registries and keep your image there.


# How Docker work behind the scenes?
Before jumping into how Docker works, we need to understand how a process communicates with the hardware

Processes cannot talk to the hardware directly. They have to communicate with the **Kernel**, which works as the bridge between hardware and software


![pic1.drawio.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1662142241161/oLgWNuYdG.png align="left")

The process uses a **system call** to talk with the Kernel. The *system call* is the API that can be used by external programs to communicate with the Kernel.


Now, let's say you have two processes, *Spotify* and *VSCode*. For the sake of this example, let's assume Spotify requires *Node v12*, while VSCode requires *Node v16*. Let's also assume that we cannot install multiple Node versions on the same machine. So in such a scenario, one of the applications will work and the other won't. This is where Docker comes into play. Look at the below image (if the image is not clear, please right-click on the image and open the image in a new tab)


![pic2.drawio (1).png](https://cdn.hashnode.com/res/hashnode/image/upload/v1662143024547/N8gBlojEv.png align="left")

Here, Docker creates isolated segments in your Hard Disk and can redirect to the proper disk segment as required by different processes. In this way, Docker helps avoid dependency and configuration issues that could've been otherwise conflicting. In this sense, we say that a containerized application can work on other computers too without any issue.


But the burning question is, how does Docker achieve this isolation?


# Namespace and Control Groups

To achieve this, Docker uses some low-level kernel features, those are *Namespace* and *Control Groups (cgroups)*

**Namespacing**: Isolating resources per process or a group of processes. For example, isolating the separate versions of Node.js in the hard disk from our above example.

** Control Groups**: Limiting the number of resources used per process. This can be used to dictate how much resource (processing power, memory etc) a containerized process can use



![pic3.drawio.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1662144045671/s8m5VT02l.png align="left")


The Docker Engine or more specifically Docker Daemon (a process named `dockerd`) is in charge of managing all these things.


But....  
There's a catch...

*Namespace* and *Control Groups* are **ONLY supported by Linux kernel**.  

Then, you might ask,  

**How Docker runs in Windows/macOS?**  
In these cases, *Docker Desktop* creates a Linux virtual machine in your said operating system and runs Docker on top of that.

Please note, that the said features in the Linux kernel are not new and have existed for quite a long time. In fact, the concept of containerization is not new either.


Now, from our previous diagram, let's mark out the containers:


![pic4.drawio.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1662145480449/I8tCwlu2n.png align="left")

In the above image, there are two containers, both marked in *orange outline*. Shows only the hard disk as the resource.


And here is a zoomed view of a single container with each resource shown:


![pic5.drawio.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1662145965792/SzNSDZ2fk.png align="left")


*One extremely important point to note, the kernel here is **independent** and expands beyond the container boundary. A single kernel is responsible for controlling multiple containers. We will get back to this later.*


# From images to containers

Let's discuss in-depth on images, and how images are actually used behind the scenes to create containers.

Images keep a *copy* (or in more formal terms **snapshot**) of the file system that is required by a specific program, along with configuration and startup command.

When you try to build a container, the following happens:

1. Look for the container image in your local system
2. If it's not found locally, reach out to Docker Hub to download the image
3. Create a placeholder container
4. Replace the placeholder container hard disk portion with the file system snapshot of the image
5. Run the startup command (and other necessary configs, if any) found in the image and start the container


![pic6.drawio (1).png](https://cdn.hashnode.com/res/hashnode/image/upload/v1662147344366/BJmVvgIr-.png align="left")


# Hypervisor vs Docker

Remember, a few blocks back I said that the kernel is independent and a single kernel is responsible to handle all container communications with the hard disk? Well, that single independent kernel is the host kernel. So if you're running Linux operating system, the main Linux kernel itself will handle all the containers.

On the other hand, a Hypervisor is an old virtualization technology (sometimes dependent on hardware support), where a whole copy of the operating system is made to run a single application. Unlike Docker, this introduces a lot of overhead, including increased boot-time. You might have used *VMWare* or *VirtualBox* with your operating system, those use Hypervisors to achieve virtualization.


![pic7.drawio.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1662147913685/7_QH-Z5dM.png align="left")

In the case of Docker, the host kernel manages everything so a complete copy of the OS is not needed. As you can already guess, the winner is Docker here. Nobody uses Hypervisor anymore unless there are specific reasons.

# Conclusion

Docker is a very powerful tool that has revolutionized how we deploy applications on the web. As a Software Engineer/Developer, it is an absolute must that you have the basic working knowledge of Docker. I hope you have enjoyed this article, if you have any questions, please use the comment section. Also, don't forget to follow and subscribe to my blog. Have a great day!



