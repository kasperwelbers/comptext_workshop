## Running your code in the cloud

This repository contains an overview of possible things to do in the hands-on part of the workshop. It is intended as complementary material to the presentation, but where possible I repeated information in case you want to read it back later.

As mentioned in the presentation, there are many different possible paths to take here, and these intersect with a wide range of skills that you would need. So what is most relevant for you to focus on right now depends a lot on your current experience and goals. I thus figured that instead of making one single workshop tutorial, I'd just string together different things that you can explore, with references to other workshop material (from myself or others) where relevant.

If you don't really know where to get started, let me know and we'll work out a plan of attack together.

# Using a online workspace

The easiest way to run your code in the cloud is to just use an online workspace. By this I mean services for running your editor in a browser, like RStudio server (R) or Jupyter notebook (Python). This way, everything works pretty similar to how you would use it locally, but with two main benefits:

- Since your actuall session is on a remote device, you can keep a process running when you close your laptop. So you can run your scraper or fit your model without worries. When you log back in, you can just continue where you left of. Sometimes you can even schedule timed scripts (e.g., daily scrapers), for instance using Jupyter Scheduler.
- The remote device could have better hardware than your laptop. This is particularly relevant when working with LLMs, because you'll want to use a good GPU.

There are broadly speaking two ways to get started with a online workspace. One is to just pay for a service, like RStudio Cloud or Google Colab (runs Jupyter notebook for both Python and R). Depending on what you want to, this can range from cheap to very expensive, depending on the hardware that you need. If you just need to run a light process, like a webscraper, you can just use a cheap option. For deep learning, you will probably need to slurge a bit on GPU time. If you want to try this out right now, I would recommend trying out [Google Colab](https://colab.research.google.com/). It comes with a generous free-tier that includes enough GPU power for working with LLMs.

The other approach is to set up a server yourself. As described below, this can be a bit of a hassle, but for setting up an online workspace this can really pay of in terms of costs. You can hook up your own desktop computer, or a server from your University, and host RStudio server or Jupyter notebook yourself. If you want to try this out right now, one way to go about it is to use a Digital Ocean droplet, as described below. But instead of creating a Droplet from scratch, you could just pick the RStudio or Jupyter Notebook image from the [Digital Ocean Marketplace](https://marketplace.digitalocean.com/category/data-science).

# Using a deployment platform

There are some things you cannot (easily) do via an online workspace. The most import thing being that you can't host an API or web application.

Before, if you wanted to do any of these things, you had to set up your own server (as explained below). But today, there are great alternatives that make it easy to host services. More than just a convenience, this comes along with great features for making your projects safe, performant and maintainable. There's quite a variety of such _deployment platforms_, so it can be a bit overwhelming to find out which one suits your specific needs. But if you find one that works for you, you'll be able to move stuff to the cloud in a matter of minutes.

One thing that you will need to keep in mind is that these platforms are often specialized for particular use cases. You don't get the same level of flexibility as you'd get by setting up your own server, and some things might not work well (or even work at all) on certain platforms. If you have a particular use case in mind and are not sure what platform might work, let me know and I might have some suggestions.

### The main thing you need to now: GitHub

As relatively easy as these platforms might be, one thing that you DO need to know a bit about is git, and specifically GitHub. Many of these platforms use GitHub to deploy your code. An more importantly, if you are not already using git in some way, learning to do so should definitely be your first priority.

We actually did a [workshop on git and GitHub](https://github.com/vanatteveldt/github-workshop?tab=readme-ov-file) before, so feel free to just switch to this workshop material if you feel this should be your priority now.

In order to use the deployment platforms, you really only need to learn the very basics of git, namelijk [creating a repository](https://github.com/vanatteveldt/github-workshop/blob/main/tutorials/creating_and_cloning.md) and [pushing your code](https://github.com/vanatteveldt/github-workshop/blob/main/tutorials/add_commit_push_pull.md) to it. The more complicated features are mostly intended for writing code in teams, but you can mostly ignore those for now if you're just working on your own code individually.

### Vercel: deploying front-end applications, and/or lightweight backends

If you just want to host a front-end application (i.e. a website), there are specialized platforms that you might want to consider. Some of these platforms also support lightweight backends. By lightweight I mean that they don't perform heavy or long-running tasks, like using AI models or webscraping. For example, a website might just need the backend to communicate with a database (e.g., to serve newspaper articles in an online experiment, or to store data donations).

The main example that I show in the workshop is **Vercel**, which supports both _serverless_ and _edge_. You can create a Vercel account for free, and this might just be all you need to run an online experiment or data collection with a few thousand participants. Like railway.app, Vercel works by linking a project to a GitHub repository, and will take care of all the complicated stuff for hosting (domain name, ssl). Furthermore, it can dynamically scale your applications, by starting up more instances to handles spikes of traffic, and scale-to-zero to save costs when there is no traffic.

Accordingly, if you know that your application / API fits the constraints, Vercel (or similar platforms) can be a great option. There is also little to provide in terms of instructions, because its just that easy. Just go to [Vercel.com](https://vercel.com) and give it a shot.

### Railway.app: effortless deployment of your GitHub respositories

Railway is a deployment platform that sits somewhere between a serverless platform like Vercel, and a virtual machine approach like Digital Ocean.

Like Vercel, setting up an application is as easy as linking up a GitHub repository. In many cases, Railway will even automatically detect how to deploy your code. For example, if you created a standard Flask Application in Python, Railway will recognize that it's a Python project, and automatically handle everything from installation to starting up the server. Railway also works on a pay-for-what-you-use basis. So instead of paying a fixed monthly fee, you only pay for how much memory, CPU and bandwidth you use.

The benefit of Railway compared to Vercel is that it's not just for lightweight backends. So if you have long-running tasks such as a webscraper, you won't run into any timeout limits. The downside compared to Vercel is that it's not as specialized for public facing applications (though you can easily use it to host both your frontend and backend). Also, there is no free-tier. The hobby plan costs you 5 bucks per month, but you can then spend these costs on compute (and buy more credit if you need it.).

The benefit compared to managing your own server, or Digital Ocean droplet (see below), is that you hardly need to manage anything. You can easily link up your GitHub repositories, and there is also a long list of [templates](https://railway.app/templates) to pick from (e.g. Jupyter lab, RStudio server, FastAPI).

Note that Railway is primarily designed for hosting a service that keeps running and is available online, like an API or web application. It is less suited for just running a script one time.

# Managing your own server

_Before you get into this, I have to warn that this route is quite involved, and perhaps not the most relevant for you. If you are just getting started with cloud stuff, I would recommend first playing around with a deployment platform like Railway.app. This will get you started doing usefull things much faster._

The main benefit of managing your own server is that it gives the a lot of flexibility. Also, it can be a relatively cheap solution, especially if you can get a (virtual) server for free/cheap from your university, or if you just hook up an old computer. This does come at the price of complexity though. If you just want a place to run your own scripts, like web scrapers or training models, this is not a big problem. But if you want to host a public API or conduct online experiments, you'll need to think about stability and security, which can be quite complicated.

A good place to start with playing around with your own server, is by getting a virtual server from a hosting provider. This can also take care of some of the complexity for you (like backups). There are several good providers that let you spin up a server in no time. I have had good experiences with Linode and Digital Ocean, which are quite comparable in terms of features and pricing. They also both seem to give you 100 bucks as a starting budget when you're new (though you will have to register a payment method).

I think DigitalOcean is a bit more beginner friendly, so let's go with that.

### Creating a Droplet

Digital Ocean refers to its virtual machines as Droplets. Once you're made an account, you can find the Droplet page in the menu, where you can click on **Create Droplet**. If you for some reason can't make an account (e.g., don't have a creditcard, paypall, etc) let me know and I'll set one up for you. (Though I will take it down after the conference)

The instructions for creating the Droplet are really nice, so I probably don't need to add much, but let me just walk through some key choices. (I will also walk you through these in the plenary part of the workshop, so this is just to refresh your memory)

- You first pick a **region**. This is the physical location where your server will be hosted. The main consideration here is typically where your main users (or you yourself) will be, but note that you also want to consider legal elements of international transfers of personal data, such as the GDPR
- The **image** is basically the software that comes pre-loaded on your server. It's kind of like how when buying a laptop it comes pre-installed with an OS, and possible some tools like Microsoft Office. All the OS options are Linux based. Personally I just use Ubuntu, so pick that to increase the likelihood that I can answer you questions. For versions, try to stick to LTS (long-term support) releases for long-term stability.
  - Note, though, that there is also a [marketplace](https://marketplace.digitalocean.com) with all sorts of ready to use cool things. The [data science](https://marketplace.digitalocean.com/category/data-science) section for instance has images for workspaces like RStudio or Jupyter Notebook, or ShinyProxy for deploying Shiny apps.
- If you have big monnies, feel free to splurge on your Droplet type and CPU options, but if you just want to try stuff go for **Basic** type and the smallest 4 bucks CPU option.
- Your droplet comes with some SSD disk included, so you don't really need to additional storage (add volume). But note that this could be usefull if you want to host some webscrapers.
- One of the best things about using a cloud provider instead of managing your own physical server, is that you typically get the option to automate **Backups**. For now you can ignore this, but if you're storing important data this is well worth the money.
- You can choose whether you want to **authenticate** with an **SSH key** or **password**. Generally speaking, SSH keys are safer. You can think of SSH keys as a way to tell the server that your current device (e.g., laptop) should have access. If you are not familiar with SSH keys, I would actually recommend following the steps, because the instructions are quite good, and SSH is good to know about. (It is for instance also the recommended way to authenticate in GitHub). But you can also pick password for now, and we can always set up the SSH key later ourselves.
- There are some additional options, like (free) extra metrics and (paid) managed databases. Metrics are always good to have, as you will typically want to know when your server (and thereby your experiment / scraper / etc.) breaks, and why this happened. As to why you'd want to add a separate database (instead of just storing data on your server), we'll cover this topic separately in the workshop.

### Connect to your Droplet

Now you can sign in to your droplet. On Mac or Linux, you can open your terminal and connect with

```
ssh root@xxx.xxx.xxx.xxx
```

Where you replace xxx.xxx.xxx.xxx with the IP address of your droplet. (or your domain name if you set one up)

### Setting up your Droplet

The good news is that you now have a Linux server to play around with! The bad news is that you are now responsible for managing a Linux server. This isn't particularly hard (compared to, say, getting a PhD), but it does require you to read up on several things.

- If you just want to use this server yourself, e.g., for webscraping, then you mostly just want to make sure that you can access it properly, and that others cannot. If you haven't done so when creating the Droplet, you'll want to set up your [SSH keys](https://docs.digitalocean.com/products/droplets/how-to/add-ssh-keys/)
- At the moment you're still login in as the root user. It is good practice to now create your own user account. Here we're entering Linux territory, and we won't deal with this in-depth here. In short, you'll want to give your user account sudo priviledges, because this will mean that your user still has root / administrative superpowers. It seems that Digital Ocean also has some good tutorials on all this stuff, so [here you go](https://www.digitalocean.com/community/tutorials/how-to-add-and-delete-users-on-ubuntu-20-04)
- While your at it, you can now also create other users. Invite your friends and family! Build scrapers together! Good times.
- Since you have your sudo user set up, it can also be good practice to [disabled your root login](https://www.digitalocean.com/community/tutorials/how-to-disable-root-login-on-ubuntu-20-04#step-2-mdash-disabling-root-login)

### Using your droplet to host stuff

Ok, so now you have a nice server that you can use to run scrapers and stuff. But if you also want to do things like hosting your own API or web application, there is still a lot more stuff to do. In order to serve API endpoints or websites to the internet, you'll typically set up a reverse proxy like Nginx. You'll also need to register a domain name, set up your DNS records, and handle your own SSL certificates for supporting HTTPS. As with the previous steps, this is not particularly difficult, but it is quite a ~~pain-in-the-ass~~ [hassle](https://www.digitalocean.com/community/tutorials/how-to-serve-flask-applications-with-gunicorn-and-nginx-on-ubuntu-22-04).

If going through these motions is your thang, I can definitely recommend it, as it does help you get a good intuition for how the web works. But again, I must emphasize that for most practical applications (and in particular on the scale and budget of academia) it is often a much quicker, easier, safer and more maintainable solution to go with a deployment platform that handles all this stuff for you.
