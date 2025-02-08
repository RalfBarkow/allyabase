# allyabase

![All your base are belong to us](https://github.com/planet-nine-app/allyabase/blob/main/site/images/all-your-base.jpg)

**allyabase** (named for an early oughts internet meme, which featured the quirkily translated game Zero Wing) is a playful jab at the BaaS (backend as a service) platforms in name, and a serious attempt at providing public backend infrastructure in form.
In addition to referencing a time when the internet was weirder, it is meant as a (playful) reminder that BaaS is a thinly veiled data collecting mechanism where you get to pay for the privelage of handing your user's habits to advertisers for them to profit off of.

allyabase wants to be a free, open source, self-hostable alternative to whatever other bases may be out there. 

### Establishing a base

If you're interested in hosting an allyabase base, but are unfamiliar with hosting in general, I recommend reading [this tutorial for setting up a minecraft server][minecraft] as it contains the concepts, and might also be more familiar to folks interested in hosting software for their friends.

An instance of allyabase is called a base. 
Like a minecraft server, bases are isolated instances of allyabase services.
Unlike a minecraft server, bases can connect to each other as users. 

This means if there's an app that consumes allyabase services, and you want only you and your friends to use it, you can create an allyabase instance and just use it, or if you want to make a globally spanning network of bases for some purpose you can do that too. 

In this way, allyabase most closely resembles the Fediverse, but with two key differences.
First, there is no ActivityPub-like protocol keeping everything together. 
The individual services all take differently shaped data, and that can be modified to suit client needs because the services perform no operations (I think, I probably left one or two in by mistake) on their data.

Second, the keys that represent a user are held by the user's device(s) instead of the server they connect with. 
This means users can change servers, and migrate data, or not, at will, and client applications can aggregate data from different bases rather than relying on a single server.

### An aside on what's all going on here

allyabase is a collection of miniservices[^1], which utilize the [Sessionless][sessionless] protocol (SLAP).
SLAP allows, among other things, for clients to authenticate without sharing any personal information like an email or password.
One of the most interesting things that this does is it lets you authenticate with the miniservices directly in the same or different ways from different clients.

At the risk of being hyperbolic, this enables interoperability[ht1].
If that were all this was about, it'd probably be enough for me to want to work on it. 
But as it happens, you can build some really cool stuff once you no longer have to worry about user's info and credentials. 

At the end of the day, though, someone's gonna have to pay to host this stuff somewhere, and for it to actually keep working as it scales, that's going to get expensive.
So what I want to do is work on getting it so that folks can run these miniservices on computers that already sit in their house like routers, and smart tvs, and then pay them a bit of the transactions that flow through them.
Because if no one is ever going to bother to make a printer that works, we can at least make it so you get five bucks a month for sitting on a table taunting you[^2].

### Overview

allyabase is a collection of seven miniservices.
Each miniservice provides at most two main functions.
Those functions will be discussed in brief here, with links to their API documentation.

[Addie][addie]: The accountant of the miniservices. Handles splitting up transactions, and signing people up for processors.

[Artha][aretha]: Lets you allocate a group of nineum from Fount for use in limited run products (think things like tickets, or media rentals).

[BDO][bdo]: Sometimes you just need to store a Big Dumb Object

[Continuebee][continuebee]: Saves and checks client state to verify local state matches expected state.

[Dolores][dolores]: Saves short-form videos, and provides a tag-based categorization system for them

[Fount][fount]: Ok, so there's this thing called [MAGIC][magic], and you don't really need to know about it, but it allows you to do the whole linking people together for transactions (both with money and not money). And along with this you can do some other rewards.

[Joan][joan]: Account recovery without having to do a bunch of stuff.

[Julia][julia]: Peer to peer associations for messaging, and coming soon for intra client assciating.

[Minnie][minnie]: A magic box for emails.

[Pref][pref]: Stored preferences (or any key/value pair really). Also allows for global preferences across clients.

[Sanora][sanora]: Super lightweight product hosting like gumroad or big cartel

### Tech Stack

allyabase is meant to be multi-language, and multi-platform. 
Anyone is free to take this doc as a reference and implement their own implementation in their own preferred language.
You may contribute that effort back to this repo for the benefit of others, or not.

The initial services in the linked repos have been written in JavaScript, because, like it or not, JavaScript is the closest we have to a _lingua franca_[ht2] in the programming world. 
The only dependencies are `express`, `sessionless-node`, and sometimes `node-fetch`.
For the datalayer, these reference implementations simply write to the file system.[^3]

For an example of a repo with multiple implementations, checkout the TypeScript and Java servers in [Continuebee][continuebee].

### Client SDKs

In the repos for the miniservices are client sdks written in JavaScript and/or Rust. 
Several of the JavaScript SDKs are also in npm.
You will also find an allyabase SDK in this repo that combines them all. 
This is a work in progress. 

### Roadmap

allyabase is meant to be _interoperable_ with any service that utilizes public key cryptography using the secp256k1 curve, so it's well within reason that things may be built adjacent to allyabase, which aren't officially part of allyabase, and this roadmap should neither be considered restrictive nor exhaustive.

* prof: pref, but for profiles

* locus: geo data

* TCB: tasks/todos/reminders/etc

* <name tbd>: Something for forms... not sure how far down the UI road I want this to go

### Running locally

##### Docker

From the root of the project run the build script: `build-and-run.sh` or the following commands.

`docker build -t allyabase .`

then:

`docker run -p 2999:2999 -p 3000:3000 -p 3001:3001 -p 3002:3002 -p 3003:3003 -p 3004:3004 -p 3005:3005 -p 3006:3006 -p 3007:3007 -p 7423:7423 -p 7277:7277 allyabase`

This will expose the services on each of these ports. 
You can then verify they're running by running the tests in `test/mocha` with `mocha addie.js bdo.js continuebee.js fount.js julia.js joan.js pref.js`

##### Testing the dev server

*NOTE:* Tests are currently broken in this repo as I have to move some things around. 
I wasn't quite ready to share, but things got a little weird, and timelines had to be shortened a bit.

You can run the local tests against the dev server with `DEV=true mocha addie.js bdo.js continuebee.js fount.js julia.js joan.js pref.js`

### Contributing

Each miniservice has its own contributing guide. 
Should you want to make a contribution to a service, you should check there.

If you'd like to add an allyabase client, you should submit a PR with a video of you testing all the methods of the client against the dev instance of allyabase.

### Open Source Force

This project would not be possible without the support and contributions of the excellent people of [Open Source Force][osf].

[sessionless]: https://www.github.com/planet-nine-app/sessionless
[magic]: https://www.github.com/planet-nine-app/magic
[addie]: https://www.github.com/planet-nine-app/addie
[aretha]: https://www.github.com/planet-nine-app/aretha
[bdo]: https://www.github.com/planet-nine-app/bdo
[continuebee]: https://www.github.com/planet-nine-app/continuebee
[dolores]: https://www.github.com/planet-nine-app/dolores
[fount]: https://www.github.com/planet-nine-app/fount
[joan]: https://www.github.com/planet-nine-app/joan
[julia]: https://www.github.com/planet-nine-app/julia
[pref]: https://www.github.com/planet-nine-app/pref
[sanora]: https://www.github.com/planet-nine-app/sanora-dot-club
[juliachat]: https://www.github.com/planet-nine-app/JuliaChat
[osf]: https://opensourceforce.net
[minecraft]: https://minecraft.fandom.com/wiki/Tutorials/Setting_up_a_server

[ht1]: ## "In the hierarchy of non-centralized systems it goes decentralized (like blockchain), then federated (like the Fediverse), then interoperable (like cellular networks, or the world wide web). Interoperability was actually the norm in the years before the internet. You don't have to have different pens for different notebooks."
[ht2]: ## "A bridge language. Like common in DnD."

[^1]: "I avoid using the term microservices here because a) they've sort of fallen out of favor, and b) because the allyabase miniservices (and miniservices that may be outside of allyabase) are much more stand alone because of how auth works for them."
[^2]: "The potential socioeconomic implications of a public cloud of this sort will be discussed in another doc soon."
[^3]: "I originally started out with Redis, but Redis isn't free, and so that kind of defeated the purpose of using it. After doing some more digging for a database solution, I was leaning towards sql-lite, but after feeling how lightweight everything was, it felt like adding a bunch of steps to install a database here and there undermined the spirit of the endeavor."
