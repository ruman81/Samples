﻿# RabbitMQ Connector Sample App - RabbitMQConnection

ASP.NET Core sample app illustrating how to use [Steeltoe RabbitMQ Connector](https://github.com/SteeltoeOSS/Connectors/tree/master/src/Steeltoe.CloudFoundry.Connector.Rabbit) for connecting to a RabbitMQ service on CloudFoundry. This specific sample illustrates how to use a `RabbitMQ.Client` to send and receive messages on the bound rabbitmq service.

## Pre-requisites - CloudFoundry

1. Installed Pivotal CloudFoundry 1.7+
1. Optionally, installed Windows support
1. Installed RabbitMQ CloudFoundry service
1. Install .NET Core SDK

## Create RabbitMQ Service Instance on CloudFoundry

You must first create an instance of the RabbitMQ service in a org/space.

1. cf target -o myorg -s development
1. cf create-service p-rabbitmq standard myRabbitMQService

## Publish App & Push to CloudFoundry

1. cf target -o myorg -s development
1. cd samples/Connectors/src/AspDotNetCore/RabbitMQ
1. dotnet restore --configfile nuget.config
1. Publish app to a directory selecting the framework and runtime you want to run on. (e.g. `dotnet publish -f netcoreapp2.0 -r ubuntu.14.04-x64`)
1. Push the app using the appropriate manifest. (e.g. `cf push -f manifest.yml -p bin/Debug/netcoreapp2.0/ubuntu.14.04-x64/publish` or `cf push -f manifest-windows.yml -p bin/Debug/netcoreapp2.0/win10-x64/publish`)

> Note: The provided manifest will create an app named `rabbitmq-connector` and attempt to bind to the the app to RabbitMQ service `myRabbitMQService`.

## What to expect - CloudFoundry

After building and running the app, you should see something like the following in the logs.

To see the logs as you startup and use the app: `cf logs rabbitmq-connector`

On a Linux cell, you should see something like this during startup:

```bash
2016-08-24T12:22:42.68-0400 [CELL/0]     OUT Creating container
2016-08-24T12:22:43.04-0400 [STG/0]      OUT Successfully destroyed container
2016-08-24T12:22:43.95-0400 [CELL/0]     OUT Successfully created container
2016-08-24T12:22:54.43-0400 [CELL/0]     OUT Starting health monitoring of container
2016-08-24T12:22:56.64-0400 [APP/0]      OUT Project app (.NETCoreApp,Version=v1.0) will be compiled because expected outputs are missing
2016-08-24T12:22:56.67-0400 [APP/0]      OUT Compiling app for .NETCoreApp,Version=v1.0
2016-08-24T12:23:00.57-0400 [APP/0]      OUT Compilation succeeded.
2016-08-24T12:23:00.57-0400 [APP/0]      OUT     0 Warning(s)
2016-08-24T12:23:00.57-0400 [APP/0]      OUT     0 Error(s)
2016-08-24T12:23:00.57-0400 [APP/0]      OUT Time elapsed 00:00:03.9012539
2016-08-24T12:23:01.72-0400 [APP/0]      OUT
2016-08-24T12:23:02.81-0400 [APP/0]      OUT Hosting environment: development
2016-08-24T12:23:02.81-0400 [APP/0]      OUT Content root path: /home/vcap/app
2016-08-24T12:23:02.81-0400 [APP/0]      OUT Now listening on: http://0.0.0.0:8080
2016-08-24T12:23:02.81-0400 [APP/0]      OUT Application started. Press Ctrl+C to shut down.
2016-08-24T12:23:02.89-0400 [CELL/0]     OUT Container became healthy
```

At this point the app is up and running. To send a message click "Send" and send a message over RabbitMQ. Having sent a message, click "Receive" and you will start seeing those messages.

---

### See the Official [Steeltoe Service Connectors Documentation](https://steeltoe.io/docs/steeltoe-service-connectors) for a more in-depth walkthrough of the samples and more detailed information