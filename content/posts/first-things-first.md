---
title: "First Things First"
date: 2020-02-22T07:54:19-07:00
draft: true
---

### Containerize It

Now that I have a basic Idea of what I want to build, it's time to dive in.

The very first thing that I want to do is create a Phoenix app and throw it in a Docker container.
After creating the Phoenix app by following their getting started guide, I created the following Dockerfile:

```Dockerfile
FROM elixir:latest

RUN apt-get update && \
  apt-get install -y postgresql-client

RUN mkdir /app
COPY ./remote_coach /app
WORKDIR /app

RUN mix local.hex --force

RUN mix do compile

CMD ["/app/entrypoint.sh"]
```

As you can see, it's very straight forward.
Just copy the code, compile, and then call the `entrypoint` script.
Now, Phoenix needs a database (usually postgresql).
I could either run a local instance of postgres, or I could use a container. 
Since this is for development for now and I'm not too worried about data loss when a container might crash, I went with a container.
For now, I am just using `Docker-Compose` to spin up these two containers because it was the quickest thing for now.
I plan on using Kubernetes later to orchestrate these containers. 

Here is the simple compose file that I am using:

```yaml
version: '3'

services: 
  phoenix:
    build:
      context: .
    environment:
      PGUSER: postgres
      PGPASSWORD: postgres
      PGDATABASE: remote_coach_dev
      PGPORT: 5432
      PGHOST: db
    ports:
      - "4000:4000"
    depends_on:
      - db
  db:
    image: postgres:12.1
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      PGDATA: /var/lib/postgresql/data/pgdata
    restart: always
    volumes:
      - pgdata:/var/lib/postgresql/data/pgdata

volumes:
  pgdata:
```

From there it's just a simple
```bash
docker-compose build
docker-compose up
```
and we are up and running. 


### Basic AWS VPC
