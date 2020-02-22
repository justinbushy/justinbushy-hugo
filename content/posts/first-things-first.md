---
title: "First Things First"
date: 2020-02-22T07:54:19-07:00
draft: false
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

The next thing that I need is an AWS environment that I can build as I go. 
I plan to build this application in a way that my CI/CD pipeline can deploy code from my master branch.

For my initial test VPC, I went with a very simple 2 subnet setup in the same AZ.
For now, this is all I need to just get started and play around.
I have one public subnet and one private.

To create this VPC, I went directly to CloudFormation. 
This is to keep as much infrastructure as possible defined in code and to utilize a lot of the benefits of what AWS has for IaC.

The CloudFormation stacks that I wrote to create the VPC are too long to post here, but you could see them in my test bed project on [Github](https://github.com/justinbushy/aws_test_bed/blob/master/cloud_formation_stacks/test_vpc.yml)


### Saving Dollars

This is kind of a side note, but now that my AWS account is out of the free-tier range, I am starting to see how expensive even the smallest things can be.
One of these expensive things is an AWS NAT Gateway.
Just to have a NAT Gateway on 24/7 with no data costs $32/month.
While that is a small price to pay for a managed gateway, I figured I could save some money since I won't be using this 24/7 at this point.

AWS suggested a small linux instance that I manage myself for a cheaper NAT, but I thought I might be able to do something more clever for now.
I decided to try using CloudFormation to spin up a new NAT whenever I needed one.
It ended up being pretty straight forward:

```yaml
Resources:
  GatewayEIP:
    Type: AWS::EC2::EIP
  NatGateway:
    Type: AWS::EC2::NatGateway
    Properties:
      AllocationId:
        Fn::GetAtt:
          - GatewayEIP
          - AllocationId
      SubnetId:
        Fn::ImportValue: 
          !Sub "${VPCStackName}-Private-Subnet"
      Tags:
        - Key: Name
          Value:
            Fn::Join:
              - ""
              - - Fn::ImportValue: !Sub "${VPCStackName}-VPCID"
                - "-NAT"
```

The great things about using CloudFormation for this:
- Using Managed AWS Gateway instead of managing my own
- Deleting various resources tied to the NAT is automatic

That's all I have for now, the next update will be setting up my Drone CI/CD server.