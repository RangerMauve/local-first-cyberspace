# local-first-cyberspace
A roadmap for realizing a local-first and offline-first cyberspace

## Why?

Virtual Reality is currently comprised of a bunch of large silos. Facebook, Google, Microsoft, Valve, and other such companies are all trying to control what people can and can't do in VR. As a user, you have to choose to buy a specific platform and limit what you can consume, and as a creator you have to jump through a bunch of hoops and pay for licences in order to create and share things.

### This sucks.

[WebXR](https://immersive-web.github.io/webxr/) flips this on it's head. Instead of targeting specific platforms, creators can create experiences using websites and have them work on any headset that has a web browser which implements the WebXR spec (all of them). You don't need to buy into a specific set of APIs on a specific platform, you don't have to worry about app stores sitting between what you created and users, and you can instead focus on making weird stuff and sharing it with people. There's some limitations in that web browsers and JavaScript add a bit more overhead and might get in the way of using the hardware to it's fullest, but I beleive it's not so high that you can't create meaningful experiences, and that the freedom people get is usually worth it.

### But it's not enough.

So, publishing content across platforms is cool, but the actual act of publishing and the interaction with this content is still limited by a key feature of the web: Creating and Hosting servers. On the surface this isn't too bad since putting stuff "on the cloud" isn't particularly hard these days, but there's some stuff that I don't like about it.

First, using servers as your building block means that you can't get stuff to work offline without some really clever APIs. This is seen most clearly when a cloud service you want to use goes offline, or you try to do something while offline and get opaque error messages. The web has fancy things like Service Workers to get around some of these problems, but they're really complicated and need to be opted into to get their advantages.

Secondly, scaling up centralized services is _hard_. The more users you have, the harder it is to keep stuff online and functional. This means you either need a large team of people working on the "back end", or to pay some large cloud provider to do stuff for you, giving up control and locking your application into the particular provider's services. So you either need to have money to employ clever people to keep things running, or have money to pay a corporation to do it and give up a bunch of freedom while you're at it.

Lastly, the thing that bugs me the most is that user's don't own their own data. When you use a server, it usually means that any data you want to share with others, or even share between your devices, has to be stored by the server, and accessing it is dictated by the server. If you have a social media account, the only way you can use that data elsewhere is by having developers pay to access the data through the API, and even then they're at the mercy of the corporation controlling the data and the user has zero say on the matter. It'd be nice if users could keep control over their data on their own terms, and services were granted access to it rather than the other way around.

### Local-First Software

Wouldn't it be cool if we could get rid of servers for most use-cases, and instead focus on the web part? That's the part of the premise of using [local-first software](https://www.inkandswitch.com/local-first.html). Instead of servers as your building block, you build on top of peer to peer file transfer protocols. The easiest one to use, in my opinion, is [Dat](https://dat.foundation/). When you want to create a website (vr or otherwise) you create a "Dat Archive" which is a folder you can shove your website files into. You then get a URL which you can share with others to load files from the folder. It's kinda like torrents, but it supports more files, and you can update the contents without needing to create a new archive.

So here's what building on top of P2P primitives gives you:

- You can create archvies locally without the internet, and modify them without needing to talk to a cloud
- Once you've loaded an archive, you have a copy locally and it'll work fully offline
- You can load archives through local networks over WiFi (or new network types) rather than reaching out across the world
- You no longer need to worry about scaling back-end because there is no back-end
- The more people are loading your content, the more people are re-sharing it
- A developer has full control over their software without needing a third party
- When users want to store data, they store it in an archive and give applications access to it to read/write
- Users have control over their data by default and their data is more portable
- Sharing data is private by default and only people you've shared your URL with can load your content
- Creating a copy of a website and customizing it is easy, you click a button and modify the archive contents

This is pretty great. And then you combine that with WebXR and you get:

- VR worlds that exist offline
- Content can be shared either locally or over the internet with seamless transitions
- Users can author content that exists on their own devices first, and on servers optionally
- The API surface for building experiences is smaller so there's less stuff for people to learn

## Leveraging the strengths of the web

I think the coolest part of the web is that people can dig around and reuse ideas.

Blogging platforms like Tumblr had a really cool example of that where people would customize their blogs. I think it's important to enable people using VR apps to have the ability to peek under the hood and mess with them. Dev tools should be built into VR browsers to enable people to tinker with the page similarly to what they can already do on desktop / laptop web browsers with 2D web pages.

Links are really great because they facilitate loading content from other sources, and giving users an easy way to discover more relevant information. Crawlers can be used to map the web and I think this has a lot of potential for VR. Imagine visualizing these links in 3D using visual metaphores like [portals](http://janusvr.com/docs/learn/portals/index.html). Linking to resources lets you share images / videos / audio / styles between websites. This can be extended to include 3D models in VR.

Lastly, iframes are a really cool idea because they enable you to embed somebody else's website within your own website. This is pretty crazy since it's really hard or annoying to do this in different environments. Ads usually make use of this so that a website owner can leave it to the ad provider to actually generate content on their page, but there's other use cases like embedding chat boxes or other "widgets" on a page. Again, this can be leveraged in VR. [Exokit](https://exokit.org/), an engine for building WebXR experiences, recently landed support for what they call ["reality layers"](https://github.com/exokitxr/exokit/pull/760). This takes the concept of iframes and applies them to WebXR. Essentially, you can create an iframe that loads some 3D content, and then position it within your own 3D space. That way you can mix and match interactive bits of virtual reality without having to do strong integrations with the code running in those spaces. This is something you can't do in any other VR platform due to the siloing of game engines and applications themselves.

In short, the web's biggest strength is the ability for people to mix and match content from different places, and tinker with the content they load, and combining this with P2P and VR will enable people to innovate and share content without the constraints of siloed ecosystems.

## How to get there

Here's a vague roadmap of stuff that needs to be done to realize a local-first cyberspace.

- [ ] Dat support in [Exokit](https://github.com/exokitxr/exokit)
  - [x] Rework [window-xhr](https://github.com/modulesio/window-xhr/issues/5) to use fetch to consolidate resource loading
  - [x] Add intercepts to [window-fetch](https://github.com/modulesio/window-fetch) for custom protocols
  - [x] Implement [dat-fetch](https://github.com/RangerMauve/dat-fetch) which taks a DatArchive instance and exposes fetch. Based on [dat-fetch](https://github.com/RangerMauve/load-dat-page/blob/master/XHRPatcher.js#L69)
  - [x] Modify the `fetch` at the [top level of exokit frames](https://github.com/exokitxr/exokit/blob/master/src/WindowBase.js#L23), and the one used for [synchronous script loading](https://github.com/exokitxr/exokit/blob/master/src/request.js#L20)  to make use of the dat-enhanced fetch. Use [node-dat-archive](https://github.com/beakerbrowser/node-dat-archive) to start.
  - [x] Test loading content from `dat://` URLs
  - [ ] [DatArchive API](https://beakerbrowser.com/docs/apis/dat) available to JS
  - [ ] [datPeers](https://beakerbrowser.com/docs/apis/experimental-datpeers) or [PeerSocket](https://github.com/beakerbrowser/beaker-core/pull/6) API for multiplayer. Use [dat-peers](https://github.com/RangerMauve/dat-peers) module to start.
  - [ ] Integrate [dat SDK](https://github.com/datproject/sdk)
- [ ] Dat support in regular web
  - [x] Loading content as websites: [dat-gateway](https://github.com/garbados/dat-gateway/)
  - [x] DatArchive API: [dat-archive-web](https://github.com/RangerMauve/dat-archive-web)
  - [ ] URL rewriter on pages / intercepting `fetch()` calls. Progress: [load-dat-page](https://github.com/RangerMauve/load-dat-page)
  - [ ] Make it easy to deploy [homebase](https://github.com/beakerbrowser/homebase/) on own computer / cloud instance
 - [ ] Dat in other browsers
   - [x] Extensions: [dat-webext (Firefox developer edition, with flags disabled)](https://github.com/cliqz-oss/dat-webext)
   - [x] Desktop: [Beaker](https://beakerbrowser.com/), [Cliqz](https://cliqz.com/en/latest) (uses dat-webext)
   - [ ] Mobile: [cliqz-concept-browser](https://github.com/cliqz/cliqz-concept-browser) (uses dat-webext, not yet released), [Bunsen (WIP)](https://bunsenbrowser.github.io/#!index.md)
   - [x] Chrome OS: [DatPart](https://github.com/HughIsaacs2/DatPart)
- Dat support in frameworks
  - [x] P2P Multiplayer: [aframe-dat-peers-networking](https://github.com/RangerMauve/aframe-dat-peers-networking)
  - [x] [JanusVR](https://github.com/jbaicoianu/janusweb) for loading scenes
  - [ ] Mozilla Hubs?
- [ ] Interactions in VR
  - [ ] Navigating worlds / loading bookmarks
  - [ ] High level portal component using the iframe feature in exokit for aframe. `a-iframe`?
  - [ ] Terminal for editing the DOM [dat-xr-scene-ide](https://github.com/RangerMauve/dat-xr-scene-ide/)
  - [ ] Sketch up set of interaction types in VR. E.g. touch/grab/activate
  - [ ] Create base VR scene with useful interactions like physics
  - [ ] Tools for moving objects / adding new ones in VR
  - [ ] Create a module for saving an aframe scene to an HTML file after it's been modified
  - [ ] Add ability to spawn in tools for modifying the world when in archive and save the world after editing
  - [ ] Object templates and components for spawning them
  - [ ] Create high level scripting for interactions in VR (on click, create new object from template; on collide with player, get bigger)
  - [ ] Graph editor for creating interactions. Inspired by stuff like [Glitchspace](https://store.steampowered.com/app/290060/Glitchspace/), or [The Magic Circle](https://www.magiccirclegame.com/seriously)
- [ ] local-first cyberspace Operating System
  - [ ] Create high "OS" based on Exokit focused on P2P WebXR
  - [ ] Root some existing standalone headsets like the Go, Quest, or Magic Leap
  - [ ] Install Exokit with the defeault envrionment as the launcher
  - [ ] Experiment with getting Ad-Hoc Mesh networking with the WiFi adapter.
  - [ ] Integrate mesh networking with Dat stack in exokit
  
 ## Contributing
 
 What's cool, is that a lot of the goals of local-first cyberspace are aligned with WebXR or Dat in general!
 If you're interested in moving this along, feel free to [create an issue](https://github.com/RangerMauve/local-first-cyberspace/issues/new) with either ideas on what could be done, or pick from anything on the roadmap that seems like fun. If nothing else appeals to you, consider creating some sort of VR content and publishing it to the P2P network with [Beaker](https://beakerbrowser.com/).
