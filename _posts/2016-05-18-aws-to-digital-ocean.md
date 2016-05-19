---
title: "Moving from AWS to Digital Ocean"
layout: default
---

# Moving from AWS to Digital Ocean

> This post explores my history of using the cloud, from early experiments through 7 years of AWS, and finally the decision to look at a new provider

## First steps into the cloud

When we first launched we started out running on a single dedicated server provided by ServePath. As customer numbers started to grow we augmented this with a number of virtual instances hosted on GoGrid. GoGrid was just one of a bunch of different cloud infrastructure products which started to flood the market at that time, and in this case a logical choice because they were a ServePath product, and could be easily networked to the dedicated instance.

It quickly became clear that GoGrid at the time was really trying to bootstrap itself before they had got a really comprehensive product in place. Problems included a lack of OS images - the dedicated server and all my development systems were running Ubuntu 8.04, but the only options on GoGrid were Red Hat or CentOS. In the days before Puppet and Chef this made configuration scripts extremely complex, and made even simple sysadmin tasks like checking log files and installing security updates a pain. Many options like networking which seemed simple on the face of it turned into massive time sinks, and even getting the server clocks to run to time was a nightmare. 

In the end the clincher for us was a persistent networking problem which was causing periodic massive packet loss on the internal network. Every time we raised this with their increasingly poor customer support we were told that they had run diagnostics and everything was fine. When we finally challenged them to provide the output of the diagnostics it turned out that their tests were simply invalid. We were able to get them to identify and switch out a faulty component, but only after losing days of engineering time and a month of issues affecting customers. 

It's quite likely that GoGrid eventually managed to get on top of their issues and provide okay service now, but for us in 2009 it just wasn't viable anymore. We needed to move.

## Moving to AWS

I had been experimenting with AWS for a while at this point, running a server there in order to speed up service in Europe (getting to our West coast servers from the UK at the time could add 250ms latency to a connection at the time). My impression was that they were fiddly-to-use and rather expensive. The pricing was basically on a par with dedicated servers from smaller providers, and control was either through the API or through a flaky Firefox extension. 

On the other hand, they had 3rd party Ubuntu instances available, and they seemed to be a fair way ahead in terms of uptime and good configuration. Fundamentally they were more difficult to get started in the first place, but you could be reasonably confident that once they were running you wouldn't have to spend hours trying to figure out what was going on.

Luckily for me, around the time of the move we hired our first sysadmin, so I was less involved in the day to day work of keeping it running. A lot of my job with EC2 instances from there on out was to sit back and watch the prices slowly decline.

## AWS Improves

Okay, in prodcution terms I may have stopped being the one who had to fire up instances and keep them running, but I have been in charge of managing the costs and archiecture as the stack grew. That involved staying on top of all the awesome service-level developments in AWS over the period - we've benefitted heavily from replacing HAProxy with ELBs, installing Redshift before we ever had to look at more expensive analytical database options, using Route 53 and Cloudreach. As you slip more and more of those into the mix it gets harder to even think about shifting to another full-serice cloud like Google, let alone going back to another bare-bones provider or, god forbid, owning your own physical servers.

I've also seen the tooling grow and mature. While using the AWS console can be painful for bleeding edge new features, and changing anything in the security model can leave you feeling like you are stepping on shards of broken JSON, it is absolutely fine for the basics. In particular, if you are just trying to fire up compute instances and control access to them it lets you do everything you need completely painlessly.

## Getting back into the game

As things mature and get more complex, it's useful as somebody who likes to code to solve problems that you have a couple of personal projects on the go. These give you the chance to have a noticeable impact on something with just a few hours work, and help to remind you of why you got into this in the first place. And as somebody who has pretty exclusively coded for the web throughout my career, that inevitably meant that I needed servers. Well, strictly-speaking I could have gone with a platform solution like Lambda, Heroku or Compute Engine, but I like to have the freedom to play with a lot of different things at once, including firing up old projects, and for that nothing beats a general purpose server.

Luckily by this time Amazon had launched their early micro instances. If you are just going to be playing at weekends, and don't have to worry about users coming along and eating up your processor with their incessant requests these are just the job. Using my extensive knowledge of random clicking I was able to fire up a server and get something I could host websites on in a few minutes, and just as well since a few minutes is pretty much all I had. Working through from the original micro instances to the T2 family this served my needs well for several years.

## Going over the top

Unfortunately a more recent project needs a little more power. Not much more power, but basically it has to do a bunch of processing on startup, and startup is something that is done quite a lot in the development phase. This meant that I was starting to run out of burst credits on the T2 instance. It was time to move to something more substantial.

This growth isn't the huge issue it was a few years ago. The larger server type I'm looking at would still cost maybe 15% of what our massively less powerful dedicated server cost in 2007. On the other hand, it's enough to become a line-item, something I might actually notice on my monthly bank statement. So I decided to look into other options.

The most obvious competitors to look at was Google Cloud Compute and Azure. Both of these which came too late for us as a company. By the time Google and Microsoft realised that they needed to move on from PaaS into IaaS we were pretty wedded to the AWS ecosystem, so their only real impact was in driving down cost. This was perhaps a chance to review other options and make a different choice this time.

I understand that Azure are making ever more strides into the open marketplace now, with simple effective servers running non-Windows operating systems. My previous experience though is that dealing with Microsoft puts you into a whole different sphere of development, where Windows and Visual Studio still rule the roost, and you will be gradually assimilated. If I was buying a few hundred servers I might do a detailed analysis rather than falling back on gut reaction, but for a couple of personal servers I'm happy enough to risk missing out.

Google Cloud Compute were more interesting. They are really competing hard on price, and in particular have a good solution to the problem which has always bothered me on AWS: reservation. Basically in a fast-moving environment it is difficult to commit to a particular server for a year or three. Every time I did it it seemed like the prices would be cut the following month, or a new class of servers would be made available which better fit our needs and we would be tied to the old ones for months. I know Amazon have made great strides on this, but compared to Google's proposition of discounts if you turn out to have used the server all month they are really nowhere. The prices were good too, and I'd heard great things about the tooling. GCE was definitely high on the list of options.

But what about the others? Many of the small infrastructure providers from the boom time of late last decade didn't make it through, but a couple of names stuck out as currently providing IaaS. The first of these was Digital Ocean. I'd heard good things about them from a price point of view, they were starting to develop interesting partnerships such as the one with [Gitlab](https://about.gitlab.com/2016/04/19/gitlab-partners-with-digitalocean-to-make-continuous-integration-faster-safer-and-more-affordable/), and best of all they had servers in the UK, so I could dump the (by now pretty small) latency of a transatlantic round trip.

A quick check on price confirmed what I needed to know - whatever the quality I wouldn't be paying any more. Essentially for exactly the same price as T2 Small instance on AWS I could get the same amount of memory and non-burstable compute. The choice was made and it was time to get started.