---
layout: post
author: jan_de_wilde
coauthor: steve_de_zitter
title: 'JS Conf Budapest Day 2'
image: /img/js-conf-budapest-2016.jpg
tags: [JSConfBudapest,JavaScript,Conference]
category: conference
comments: true
---

## From JS Conf Budapest with love

This years JS Conf Budapest is hosted at [Akvárium Klub](http://akvariumklub.hu/).
Located right in the center of the city, below an actual pool, filled with water!

> Akvárium Klub is more than a simple bar: it is a culture center with a wide musical repertoire from mainstream to underground.
> There is always a good concert and a smashing exhibition, performance, or other event happening here, in a friendly scene, situated right in the city center. 

JS Conf Budapest is hosted by the one and only [Jake Archibald](https://twitter.com/jaffathecake) from Google.
After waiting in line to get our badges we were welcomed at the main hall where some companies hosted stands.
In another space after the main hall tables were nicely dressed and people could have a nice breakfast.
For the coffee lovers, professional baristas served the best coffee possible. With a nice heart drawn on top if it.

![Keynote]({{ '/img/js-conf-budapest/from-js-conf-budapest-with-love.jpg' | prepend: site.baseurl }}) 


****


## Day 2: Morning

* [Laurie Voss: What everybody should know about npm](#laurie-voss-what-everybody-should-know-about-npm)
* [Safia Abdalla: The Hitchhiker's Guide to All Things Memory in Javascript](#safia-abdalla-the-hitchhikers-guide-to-all-things-memory-in-javascript)

### Laurie Voss: What everybody should know about npm

Laurie is CTO at npm Inc.

You can find him on Twitter using the handle [@seldo](https://twitter.com/seldo).

> npm is six years old, but 80% of npm users turned up in the last year.
> That's a lot of new people! Because of that, a lot of older, core features aren't known about by the majority of npm users.
> This talk is about how npm expects you to use npm, and the commands and workflows that can make you into a power user.
> There will be lots of stuff for beginners, and definitely some tricks that even most pros don't know.

#### How does npm look up packages?
In contrast to what most people think, npm does not download it's modules from GitHub or other version control systems.
They would not like it that such an amount of data is transferred on a daily basis.

In short npm does this: You -> CLI -> Registry.
Let's dive in.

1. First npm will take a look at your local cache and see if the package your are looking for is present.
2. Next it will resort to the CDN network and will use a server whos is the closest to your position as possible.
3. Finally, if npm can't find the package in local cache or the CDN network, it will look up in the registry. The registry is a set of servers all around the world and will try to match the best version that you are looking for.

#### EACCESS error
A lot of people have issues with EACCESS errors because they used sudo to install things.
The easy solution is to always keep on using sudo BUT we can [easily fix npm permission issues](https://docs.npmjs.com/getting-started/fixing-npm-permissions).

#### package.json
Don't write your `package.json` yourself, let NPM do it!
It will always do it better. Use `npm init` which will ask some basic questions and will generate `package.json` for you.

#### Scopes
A new feature in npm is scopes.
These are modules that are "scoped" under an organization name that begins with `@`.
Scopes can be public and private.
Here is how to use scopes:

{% highlight sh %}
npm init --scope=username
npm install @myusername/mypackage
require('@myusername/mypackage')
{% endhighlight %}

#### npm-init.js
To extend the `npm init` command it is possible to create an `npm-init.js` file.
This file is a module that will be loaded by the `npm init` command and look in this file and provide basic configurations for the setup.
By default the file is placed in root of your project: `~/.npm-init.js`.
You can use [PromZard](https://github.com/npm/promzard) to ask questions to the user and perform logic based on the answers.
Remember that `npm init` can always be re-run.

#### Why add stuff in devDependencies.
Simple: because production will install faster! A lot of people don't tend to do this, so please do this!
When using this you can simple run the command below on production and be done with it.

{% highlight sh %}
npm install --production
{% endhighlight %}

#### Bundled dependencies
One of the biggest problems right now with Node.js is how fast it is changing.
This means that production systems can be very fragile and an `npm update` can easily break things.
Using `bundledDependencies` is a way to get round this issue by ensuring, that you will always deliver the correct dependencies no matter what else may be changing.
You can also use this to bundle up your own, private bundles and deliver them with the install.

{% highlight sh %}
npm install --save --save-bundle
{% endhighlight %}

#### Offline installs
A way to prevent npm to look up the registry and ensure local installs is by adding the variable `--cache-min` and to set it to a high value such as 999999.

{% highlight sh %}
npm install --cache-min 999999
{% endhighlight %}

#### Run scripts
In `package.json` it is possible to define default run scripts as showed below.

{% highlight sh %}
npm start
npm stop
npm restart
npm test
{% endhighlight %}

Of course it is also possible to define your own run scripts.
You can run these scripts like this:

{% highlight sh %}
npm run <anything>
{% endhighlight %}

#### Run scripts get devDependencies in path
Don't force users to install global tools, that is just not cool.
This way you can prevent to get conflicts over global tools, because different projects can use different versions.

#### SemVer for packages
npm uses Semantic Versioning and is a standard a lot of projects use to communicate what kind of changes are in this release.
It's important to communicate what kinds of changes are in a release because sometimes those changes will break the code that depends on the package.
Let's take a look at an example.

{% highlight sh %}
1.5.6
Breaking Major . Feature Minor . Fix Patch
{% endhighlight %}

This is quite obvious right?

npm allows you to change the version (and adding a comment) by using the commands below.

{% highlight sh %}
npm version minor
npm version major
npm version patch
npm version major -m "Bump to version %s"
{% endhighlight %}

#### Microservices architecture
When working with a microservices architecture it is possible to work with multiple packages for your services.
This can be done by using the link function within npm. 

{% highlight sh %}
npm link <dependency>
{% endhighlight %}

Let's say we have a package named Alice and we have other packages that depend on this package. We can run `npm link`.
In packages that depend on Alice, say Bob, we simply run `npm link alice`.

#### Unpublish a package
Before the [recent events](http://blog.npmjs.org/post/141577284765/kik-left-pad-and-npm) where a package called left-pad got pulled from npm and broke the internet, it was possible to unpublish a package just like that by using `npm unpublish`.
Now this is restricted after the package has been online for 24 hours.
To really unpublish the package you will need to contact support.

A more friendly way is the use of `npm deprecated` that will tell users the package has been deprecated.

#### Keeping projects up to date
Before running `npm update` it's preferred to run `npm oudated`.
This command will check the registry to see if any (or, specific) installed packages are currently outdated.

{% highlight sh %}
npm outdated
npm update
{% endhighlight %}

By doing so you can prevent yourself from breaking the project if certain packages would not be compatible.

#### Stuff everybody should know about npm
A lot of things are avaible for npm that will make your life as a developer easier.

* Babel: Transpile all the things! JavaScript, TypeScript, JSX, ...
* Webpack and Browserify
* Greenkeeper (greenkeeper.io) is `npm outdated` as a service!
* Node Security Project: Install by using `npm install nsp -g`. Use by running `nsp check`. You can use this to check if your project contains vulnerable modules.

#### Why should I use npm?
npm reduces friction.
It takes things you have to do all the time and makes things simpler and faster.


****


### Safia Abdalla: The Hitchhiker's Guide to All Things Memory in Javascript

Safia is a lover of data science and open source software.

You can find her on Twitter using the handle [@captainsafia](https://twitter.com/captainsafia) or on her webpage [safia.rocks](http://safia.rocks/). 

> This talk will take beginners through an exploration of Javascript's garbage collector and memory allocation implementations and their implications on how performant code should be written.
> Attendees will leave this talk having gained insights into the under-the-hood operations of Javascript and how they can leverage them to produce performant code.

#### Slides and interactive tutorial
The slides of this talk can be found on [safia.rocks](http://safia.rocks).
Safia also created an [interactive tutorial](https://nbdev.surge.sh/#/gist/21885286a207c05bf1194a35490420c1) on how to use the Chrome DevTools for memory management.

#### Why should I care about memory?

1. It forces us to be (better) more inventive programmers, adds restrictions and forces us to use the best tools to create the best possible experience.
2. Memory is scarce. A lot of people still use devices that are not packed with a lot of memory.
Not everyone has high performant development machines.
3. It helps us exercise our empathy muscles.

#### What does it mean to manage memory?

The Good, The Bad, The Ugly

#### How does JS manage memory?
Safia focuses on the V8 JS Engine.

We have basic types in JavaScript:

* booleans
* numbers
* strings

Memory is allocated in a heap structure and uses a root node which has references to other ones: booleans, string, etc.
So basically: root node -> references -> variables.

V8 performs memory management in 6 spaces:

* New space: memory gets allocated here when an object is created immediately.
* Old pointer space: if you don't use an object for a while, it will move to this space.
* Old data space: will end up here if you're not dealing with a reference or pointer.
* Large object space: Used to store large object tables.
They get stored here so it doesn't conflict with the store space of the above mentioned spaces.
* Code space: ...
* Map space: 

V8 uses a 'stop the world' technique that enables it to run a short garbage collection cycle.
This means it will litteraly halt the program.

V8 has different approaches on how it collects garbage in the new and old space.

* New space: Garbage collection by using a scavenging technique.
Each scavenging cycle will go through the entire heap starting from the root and will create copies.
It will clear out what is currently in new space.
Everything that is not reachable will be cleared out of the space.
You need double the size of the memory that is available for the new space to use for the copy.
* Old space: Mark and sweep technique.
Remove unmarked objects on a regular basis.

#### How do I write memory performant applications?
Asking yourself the following two question will get you started!

* How much memory is my application using?
* How often do garbage collection cycles occur in my application?

Of course you need to have the tools to work with.
The Chrome DevTools HEAP allocation profiler will be our weapon of choice.
It allows you to check the retain size and shallow size of objects.

* Shallow size of an object is the amount of memory it holds of itself.
* Retain size is all of it's size and it's dependants. 

#### Heapdump
Heapdump takes a snapshot of your heap at a specific moment.
It will provide a file with .heap extension which enables you to load it in the Chrome DevTools for further inspection.

`npm install heapdump`


****


### Yan Zhu (@bcrypt) : Encrypt the web for $0

**Is the web fast yet?**

Yes. Size of pages is rising. Amount of HTTPS requests is also rising!

**Is TLS fast yet?**

Yes. Netflix is going to secure streams this year over HTTPS.

* 2015: Netflix and chill
* 2016: Netflix and HTTPS and chill

https://istlsfastyet.com

Let's Encrypt (https://letsencrypt.org). At this moment in beta.

https://gethttpsforfree.com

### Dennis Mishumov : Why performance matters

- Speed! 1 second gain will increase revenue bij 1% for Company X. 1 second slower will decrease conversions by approx 5%.

**Performance is about perception! Not mathematics.**

The 20% rule.
- Event: make page load at least 20% faster, otherwise they don't notice. We're talking about noticeable difference. A big difference with meaningful difference.

**Noticeable !== Meaningful**

Perception: we did a live test on the conference where the sames page loaded first in 1.6 seconds and afterwards 2 seconds. Most of the people thought the second 2 second page was faster. Perception!

When delaying audio on a video, our mind will trick us by syncing the audio with what is visible on the screen. Perception!

## Day 2 afternoon

### Princiya Sequeira (Zalando - @princi_ya @ZalandoTech) : Natural user interfaces using JavaScript

**Typed, Clicked, Touched, ?**

**Typed, Clicked, Touched, Guestures/Speech/...**

Evolution of user interfaces

* CLI: Codified, Strict
* GUI: Metaphor, Exploratory
* NUI (Natural User Interfaces): Direct Intuitive

NUI: more natural and more intuitive

**NUI + JS = NUIJS**

Motivation: Started with @princi_ya trying to build simulator for motion controlled 3D camera's. Tool is not dependent on any platform. Once the simulator whas made, trying to build some apps (using leap motion for example) to move a slideshow or other purposes.

Augmented Reality.
Virtual Reality.
Perceptual Computing: bringing human like behaviour to devices
A lot of devices available: VR, motion, ...

**What next?**

Architecture:

* Step 1: USB controller reads sensor data
* Step 2: Data is stored in local memory
* Step 3: Data is streamed via USB to SDK

The streaming part is a very important part. Why? I have no clue.

https://github.com/nuijs

Open source tools also: Webcam swiper and JSObjectDetect

Example: Drawing board with a brush. NUIJS will translate the input data from the mousepointer to the Node.js Web Socket server and this one will process the data and send it back to the LEAP motion SDK. The same code can be used with the LEAP motion itself.

**Viola Jones Algorithm**

* HAAR feature selection
* Creating an integral image
* Adaboost training
* Cascading classifiers

### Maurice de Beijer (@mauricedb) : Event-sourcing your React-Redux applications

React Tutorial (Kickstarter) : @react_tutorial

**"The biggest room in the world, is the room for improvement - Anonymous**

**Restfull way**

Database <= CRUD => Server <= HTTP => Browser / React

Fine if you have a simple application.

Command query responsibility segregation

* Database => Read => Query service <= HTTP =>
* => Browser / React
* Database <= Update <= Command service <= HTTP =>

Note: 2 lines above need to be stacked (in flow form) and both end up to Browser / React

Event sourcing will not overwrite the previous state, but will save the new state. This means that you get a backlog or version history.

### Rachel Watson (@ohhoe) : The Internet of Cats

How can we incorporate cats in technology?

**Trying new things is scary**

* Embarking on a new project: will it succeed, will it suck?
* Using new technologies for the first time: what will happen, will it work for me?
* Contributing to Open Source: putting yourself out there is terrifying!

**Why so scary?**

* Fear of rejection
* Imposter Syndrome
* Inclusiveness of Communities
* Bad behaviour in General: e.g. Oh you didn't know about THIS?, e.g. completely ignoring contributions
* Your GitHub **green** timeline is not a representation of what you're worth. Just opening a PR just for the sake of it sucks.
* Don't instult the contributor, why on earth ...
* Vulgar and brutal harassment of the community, seriously, get a life!
* PR's that get ignored (for over a year) and then the maintainer writes the same fixes and says: Oops! 

**Echochamber.js**

**Proposals for new contributors**

* Find something you are passionate about
* Somthing new you want to try
* Make something cool and open source it yourself
* First point of contact is your peers
* Constructive criticism!

**Building a cat feeder bot**

Node based cat feeder that works over the web. Robokitty!

https://github.com/rachelnicole/robokitty

http://imcool.online/robokitty/

Node.js + Johnny-five + socket.io + Arduino-Uno (no internet connectivity, so not used) + Arduino Yun (not compatible with johnny five, so not used) + Particle Photon (we have winner!à

* Dry goods dispenser
* Particle Photon kit (with breadboard)
* Continuous servo
* 4xAA battery pack with on/off switch
* Misc hardware accessories: ...

A servo needs external power, so yeah, plugging it in the microcontroller is not enough. Lessons learned!

No idea how to solder, oh, worked out!

**Lessons learned**

* Don't be afraid of the unfamiliar
* Don't be afraid to ask for help
* People really like cat stuff
* Don't downplay your abilities: I mean, it's a super cool kitty food dispenser!
* I like nodebots a lot

### Nick Hehr (@HipsterBrown) : The other side of empathy

#### Empathy

Nick Hehr shares Rachels' point of view on the sometimes rude Open Source communication and communication on Social media in general.
In his talk, he addressed the way you should behave when volunteering to contribute or when giving feedback to contributors in Open Source Projects.
And Empathy turns out te be key in this process.

#### Ranting

It's all to easy to judge or express prejudice these days, through these social media channels and not think about the people who are actuall behind the idea or concept you're judging.
People that decide to Open Source the work on which they've spend tons of effort (usually because it's their passion, but still...) aren't exactly waiting for trolls or rants from people who like this easy judging.

Empathy also plays a huge role in the other way around. 
It happens al to often that people trying to contribute to OSS for the first time are being ignored (by literaly ignoring their pull requests for example), being treated like idiots (instead of being given constructive feedback in case of room for improvements), etc...

#### Saying nice things

"If you don't have anything nice* to say, don't say anything at all!"

Nice in this context means constructive. Comment on something you think could use improvement and offer a solution.
Compliment on certain aspects that really improve the tool.

Due to the relative anonymity of social media and other communication channels, we tend to forget these principles.

#### Key take-aways

Key points to take away from this session are:

- Give constructive feedback!!!
- Always keep in mind the language your using when commenting on Open Source initiatives
	- Don't be to blunt or direct in your reactions. 
- Use the right channels for your communication 
	- meaning, don't ask for feedback on twitter
	- Instead turn to platforms such as Slack, IRC, Gitter...
	- Get (constructive) feedback from people you trust
- People that open source their tools don't owe you anything. 
	- They're not entitled to give up all their time for you. 
	- They're not here to start fulfilling all requests from a demanding userbase. It's open source, submit a pull request

Living by these rules will make the (web-)world a little bit of a better place, but won't prevent other people from still continuing these bad habbits.
Don't let these people get to you! Continue doing what you're passionate about en seek out those who will give you that constructive feedback.

## Conclusion

As you can see it is fairly simple - just 4 steps - to add TypeScript support to your Ionic project by changing the default gulp setup used by Ionic.
It's nice to know that Ionic 2 will have support for TypeScript built in so you won't have to do it yourself.
By adding a flag `--ts` to your Ionic 2 project setup it will be enabled.

Personally I love using TypeScript and will use it whenever I can.
It makes my life as a developer a lot easier by spotting errors before I even hit the browser.

What are your thoughts about TypeScript? Feel free to add them in the comments section.