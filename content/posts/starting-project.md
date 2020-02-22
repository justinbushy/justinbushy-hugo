---
title: "Starting a New Project"
date: 2020-02-22T07:16:55-07:00
draft: false
---

### Time To Play Around

For a while, I've been messing around here and there with different things that have caught my interest. 
It mostly ends up with me going through "getting started" guides and reading documentation. 
While it's something, it's usually not enough to get any depth of knowledge in the subject and I end up forgetting most of it in a couple of months. 

So, for quite some time I have been trying to figure out a project to start on that would help me gain more experience in these tools. 
The problem was that I felt like I didn't have any good ideas. 
But really, I just didn't have the _perfect_ idea. 

I wanted to build something that was:
- useful
- interesting
- needed to be "scalable"

Now that last item was the trickiest.
See, most applications don't require that last bit without a ton of users.
So at first it was hard to justify the complex architecture just for a web app that one person will use, but today I've decided to hell with justification, let's just build a cool thing in the way I want to build it.

### Things I Want To Use

For this project, the fancy tools and technologies I want to use to begin with are the following:
- Elixir / Phoenix
- Kubernetes
- Kafka (or some other messaging/streaming queue)
- Istio (or some other service mesh)
- Prometheus / FluentD / Jaeger
- Whatever other "cloud native" things strikes my fancy

My plan is to start off with a mostly "monolithic" application, break it apart when it seems to make sense, and then introduce these things in the order I've put above.

### The Idea

_drumroll_

Remote coaching video analysis. 

The idea is something that I have wanted for myself when working with my olympic weightlifting coach.
For the most part, when training, I am by myself and I send him videos remotely.
He then analyzes the video and sends me some critique and help with my form.
The problem is that we mostly do this over text and it's hard to go back and reference past videos to see how I've been improving.

Now, I will be doing some discovery in the next little while, but the general idea will be:
- upload a video
- tag it with what type of movement it is (i.e. Hang Snatch)
- get a response from the coach in either text or a video analysis from something like Coach's Eye

The tagging will be great to be able to look through videos of the same movement to see progression over time or areas that need work on.
This might also be great for a "team" where there is a common place for everyone on the team to post their videos and get feedback in a place where the rest of the team can see.
This would make the remote team trainings feel a little bit more connected. (Especially if the team is doing similar movements in the same cycle)

With this new project, I will be posting articles about the things I am working. 