---
title: "Drone"
date: 2020-02-22T09:04:05-07:00
draft: true
---

### Continuous Integration

Before I even start writing any code, I wanted to start off with a CI/CD tool.
After doing some research, I have landed on using [Drone](https://drone.io/).
At first, I will only be using Drone for continuous integration while developing, but will eventually use it for deploying into my "production" system.

### IaC All The Things

With this project, I want to start everything right away with defining all infrastructure as code.
Ultimately, I want everything to be able to be spun up with the run of a command.
So, to start out, I will be using Terraform and Ansible to create my Drone server.

