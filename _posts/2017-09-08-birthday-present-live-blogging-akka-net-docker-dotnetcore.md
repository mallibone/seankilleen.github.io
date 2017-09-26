---
title: "Birthday Blogging: An Akka.NET App using .NET Core on Docker Containers"
layout: post
date: 2017-09-08 8:58:00.000000000 -05:00
excerpt: "It's a mouthful to say, but really fun to build."

references:
 - title: "Casting a Wider .NET (video)"
   url: https://www.youtube.com/watch?v=jFDXNGFvcYg
   parenttitle: ".NET Fringe Conference - YouTube"
   parenturl: https://www.youtube.com/channel/UCIz73jo7KPqVTssbUmiIfXg

comments: true
---
It's my birthday! I took the day off to treat myself, and when thinking about what I wanted to spend the day on (aided by a poll -- thanks all! TODO: Poll link), I realized -- I want to build something with some new tech. Get my muscle memory to a better place, so to speak. (Don't worry, I also like...went outside and stuff.)

I'm really excited that Akka.NET is available on .NET Core now (TODO: Blog post link) because I think it opens up some amazing scenarios for developers. And I realized that I've been a bit too busy lately to properly dive into .NET Core and Docker.

**Birthday tech goal: Acquired.** I want to get a basic Akka.NET app set up and communicating back and forth between two docker containers. Simple ping pong style event. Something like ping pong. I want to use VS Code and the command line tools to do this.

And I figured since it's new territory, I'd blog it here to go into any trials and tribulations I encounter along the way.

## Pre-requisites

#### Update docker (I already had it installed).
 * This was easy, did it through the docker UI on Windows. 
 * For good measure, ran `docker run hello-world` to make sure I was good.

#### Download .NET Core 2.0. 
 * Visited the .NET Core Site (TODO: Link) and download / run. Easy enough!

#### Create a hello world app for good measure.
* opened a prompt in powershell
* navigated to a temp directory
* ran `dotnet new console -o helloworld`

#### Update VS Code
* The 64-bit version of VS Code was out so I uninstalled my 32-bit version and installed the 64-bit. (TODO: Link)
* I open the folder in VS Code 
* I hit CTRL+SHIFT+` to open the console and notice that it immediately starts downloading / installing Omnisharp and the .NET Core Debugger. Nice!
* It also prompted me to add `launch.json` and `tasks.json`. I allowed it to add these under the `.vscode` folder.

#### Build and run my hello world app
* `dotnet build` built the file
* `dotnet run` showed me the Hello World text as expected.
* For good measure, I set a variable in the console app and added debugging, just to see. Upon running `dotnet build` I saw the compiler warning that the variable isn't used -- cool to start seeing these sorts of things from VS Code.
* Things went a little hairy here. Within VS Code I went to `Debug --> Start Debugging`, I got an error after selecting the ".NET Core" environment. I didn't catch what it was exactly, because the next time I ran it, it worked like a charm. My guess is it was an environmental configuration issue. 

At this point, I had a .NET Core app up and running and new things would work.

## Okay, Let's Do This.

I want to do the following:

* Create an Akka.NET app on .NET Core that has two actors: a `PingActor` and a `PongActor`. They will each, predictably, send messages that say "ping" and "pong" back and forth.
* I want to deploy this codebase in two different (Linux) Docker containers on different ports. This way I can see Akka.NET work across multiple machines using Akka.Remote.

#### A Quick Note: I'm Figuring it out as I go
For the parts below, if you wonder "how did he know how to do that?", the answer is: I was reading docs and googling. :) 

Let's dive in!

### Creating the Project Structure

* Within the VS Code terminal, I open a command prompt and navigate to a temp directory
* I run `dotnet new solution -o PingPong`. This creates new empty solution in the PingPong directory
* I cd into the `PingPong` directory.
* Run `dotnet new console -o PingPong.Console` which creates a folder and the console app
* Run `dotnet new console -o PingPong.Domain` which creates a folder and the class lib project
* Add the projects to the solution using `dotnet sln PingPong.sln add PingPong.Console\PingPong.Console.csproj` and `dotnet sln PingPong.sln add PingPong.Domain\PingPong.Domain.csproj`
* Reference the domain project from the console project via `dotnet add PingPong.Console\PingPong.Console.csproj reference PingPong.Domain\PingPong.Domain.csproj`
* Run `dotnet restore PingPong.sln` to update the packages
* Run `dotnet build PingPong.sln` to build the solution

Here I hit my first snag, an error: `Program.cs(9,13): error CS0234: The type or namespace name 'WriteLine' does not exist in the namespace 'PingPong.Console' (are you missing an assembly reference?)`

Spent a while trying to fix it. Turns out I named my project `PingPong.Console`, and the `.Console` meant that in the hello world console app it was confused and I had to change `Console.WriteLine` to `System.Console.WriteLine`. D'oh.
