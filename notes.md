# DOXLON 2021 July Notes

## Intro

We believe in a world where transport will be safe, green and available to all. A world where people
will be free to pursue their dreams without the need to drive.

We started out building our own high-functioning, complete autonomous vehicle system and
successfully testing it on London’s public roads. We then went on to lead the government backed
StreetWise trials, widely thought to be the most complex AV trials completed publicly in Europe. In
doing so our team began solving some of the industry’s greatest challenges.

Using this expertise, we've developed and now offer a SaaS Safety Assurance platform for autonomous
vehicle development, in which you can create or choose scenarios, run varied simulations, evaluate
the performance of your stack and configurations, refine and iterate.

We're driven by research, having  built up close collaborative links with world-class academic
groups, and our in-house research scientists and engineers publish regularly in top academic venues,
helping us drive forward the next generation of autonomous travel.

## How it started

### Pipelines

A couple of years ago, before mankind was cursed by the 'rona, I did a talk at the DOXLON 50th
  event. I talked about out ramp-up from an early stage start-up that had many irons in the fire, to
  having a well-defined product. We had a need for our software engineers to be able to put their
  software into some kind of 'production' environment. They needed their components to talk to
  others. They needed to iterate fast, and deploy services in the cloud. We had a 'DevOps' team back
  then which was the gatekeeper and holder of the arcane and mystical knowledge of the cloud. There
  were six of us to look after 150 engineers. There was no way it was going to work.

So we did a whole bunch of stuff to shift as much as we possibly could left to the component teams
themselves, so they could get on with stuff without having to wait for us. Some of it worked really
well. Some of it really didn't. This is a little story of what we learnt, and where we're going
next.

### What did we do?

- Conversations with customers
- Active engagement
    - Guild
    - Buddies
    - Training
- Self-service
    - Kubernetes as a compute platform
    - ElasticSearch, Prometheus, Sentry
    - Shared TF libraries
    - Docs, docs, and more docs so people have a quick reference and don't need to ask interrupting
    questions

Sounds a lot like a platform team, right?

### What's a platform team?!

Platforms are generally looked after by platform teams, so how do platform teams differ from
stream-aligned teams? Stream-aligned teams are, as you can guess, aligned to the value stream of
your external product. They should be independent and cross-functional, able to deliver value from
end-to-end of a particular product or domain. The goal of a platform team is to accelerate the
delivery of stream-aligned teams. One of the ways they do this is by providing a compelling internal
product that reduces extraneous cognitive load. In practical terms, this might be a tool, or a bit
of software, it could be training or consultation, a shared library, self-service APIs, or just a
wiki or a knowledge base. In reality, these internal platforms tend to be a combination of all of
these things. Sound familiar?

The primary mode of interaction between stream-aligned teams and platform teams is mostly so-called
X-as-a-Service, or producer-consumer, with defined periods of close collaboration to solve acute
problems. What that means is that rather than a stream-aligned team relying on a platform team to do
things for them, or to closely work with them over long periods, the platform team should strive to
make the stream-aligned team independent. That is, when a new capability is required, there is a
period of close collaboration during the discovery phase, we then ramp down as we establish good
practices and mature the capability, and eventually it just becomes another part of the service
offering, like any other provider you might use.


### Pulled in all directions

THe primary method of interaction for platform teams in Manuel and Matthew's definition is the
so-called X-as-a-service. So, very much like your ISP, we provide a bunch of capabilities that you
consume. We have some well defined methods for you to request new stuff, and to report broken
things. This makes it easy for the platform team to iterate independently from the value stream of
the rest of your organisation and react to their needs, rather than get bogged down as a dependency
problem in the middle. That's the theory.

If you remember back to our conversations with our customers, what they needed was a lot of hands on
help at this early stage. Delivering something as a service, where your primary mode of interaction
is via some well defined interface like bugs, incidents, and feature requests in a sprintwise manner
is a very different discipline to collaborative and facilitating work.

We found that trying to handle the two at the same time with the same group of people was a
nightmare of context switching and it hit us square in the productivities. THe 'buddy work' we
talked about turned into an interrupt-driven time sink. Not that this work wasn't valuable, it's
just that the value it created came at the expense of our team's cognitive load.

Different again is what we call support work which emerges in the middle of a sprint and distracts
from the sprint goals. We found that this sort of emergent work came at a relatively constant and
predictable rate.

### Enablers

One of the many great things in the Team Topologies book, is the concept of an Enabling Team.
Enabling teams exist to bridge and close up capability gaps. They cut across all, or a subset of
stream-aligned teams and work in this collaborative or facilitative manner for defined periods of
time, to upskill other teams, or solve a specific, acute problem, with the ultimate goal of making
themselves redundant. At least for that particular problem domain.

We set up the Engineering Enablement team with the mission:

> The Engineering Enablement Team is responsible for the advocacy, and adoption of the Infrastructure Platform and its features, the dissemination of good practice, knowledge and tooling, the bridging of skills gaps, and taking the pressure off the Infrastructure Platform team with respect to deep collaboration with product teams. It aims to form a tight feedback loop with the Infrastructure Platform team, to drive positive change and continuous improvement.

This essentially replaced the 'Buddy work' and completely solved that problem... Or did it...

(Reader, it did not)

### Team Topology

We also built a discrete support role, where we had a nominated 'on-call' person and a backup,
rotated every week to monitor the Slack channel, alerts, and support board. This was their full time
role for the week. If they started to get swamped, the backup would step in to release the pressure,
and if there wasn't any support to do, they'd pick up tech debt tickets, or throw their oar in with
the sprint work.

So we ended up like this (diagram)

The platform team providing a service with a well-defined API, and an Enabling Team to pick up
things like advocacy, closing capability gaps and being our ear to the ground for developer
experience. Brilliant.

### Utopian

So, in summary we had:

- Infra guild to spread interest, and talk about new and exciting things
- Enablement team to collaborate, advocate, and close the skills gaps
- Amazing and comprehensive documentation for excellent reference and bootstrapping
- Self service all the things
- Dedicated support channel

This is where all the blog posts end. We did the thing from the book and everyone was happy.
Productivity was 10x woo.

We all know the world doesn't work like this.

## How it's going

The original initiative that kicked off this product-oriented road to production was on very tight
deadlines for us. We had a very limited amount of time in which to get something in place so that
the product teams could iterate quickly. Unfortunately, we had to make some tough calls and
assumptions early on in the process on what was feasible for us to achieve. The ultimate consequence
of that is that we left ourselves with a lot of toil, tech debt, and fuzzy boundaries of
responsibility.

###  Premature Optimisation

We shifted too left too early.

We got the split wrong about where responsibilities lay in terms of infrastructure provisioning. We
set up repositories for each of the product teams with layered terraform modules. We would own the
bottom layers and the teams could then use the higher layers to deploy their own resources like
databases, message queues, S3 buckets etc. Some teams were absolutely fine with this and really took
the ownership seriously. Others were really not interested in learning terraform and either
completely ignored it, or ended up being a drain on our support function.

What ended up happening was a complete mess. Every team had a slightly unique way of handling things
and we also left key decisions like how to arrange and manage things like terraform modules to the
teams themselves. Consequently, when we came to help or change something, we found so many points of
fracture. We tried to consciously allow teams to make these decisions for themselves, and actually,
what they wanted was a framework or standard to follow.

### tl;dr

Writing documentation for people external to your team is hard. You make so many tiny assumptions of
prior knowledge for one thing. Even if your docs explain absolutely everything, unless you're really
diligent with cross-linking stuff, you have no assurance that someone will for example, read a page
bout how to access Vault, before they've read the page about how to auth. A solution to that of
course is to write things in a narrative way, but that assumes your user has a couple of hours to
kill reading the novel of your Kubernetes cluster.

Docs are great for reference, but not great for new users. If they aren't comprehensive enough that
the first encounter with them is a bad experience, the likelihood of someone adopting them as their
go-to thing first is very low, and you'll find yourself getting frustrated replying to Slack
messages with the same five links over and over again.

Turns out that technical writing is a skill and people do it for a living and stuff. Who knew...

One cannot survive on docs alone. They're great as a reference, but unless you have the time and
skills to invest in some really excellent documentation work, they're only ever going to be
complementary to some sort of training.

### Enablement assumed teams had time

With the Enablement team, we also optimised prematurely.

One of the things we wanted to avoid was these engagements with teams dragging on forever. What we
did was try to agree up front what the exit criteria were based on the outcomes the teams wanted.
This sort of worked. Things have ended up being a little too formal and process driven though, and I
think we lose a bit of agility and value this way. The other thing that happens is that we've only
really thought about solving the problem a team has rather than focusing on the important part,
which is leaving the capabilities behind. This is mostly an artefact though of the current pace of
delivery as we onboard new customers. We designed the process more around an organisation that was
in a steady state, rather than where we actually are which is in discovery and active development.

One problem we've faced and continue to at least to some extent, is that people don't know they
exist, or don't know what they're for despite outwardly advertising it many times over. We want to
make sure the team have enough free rein to actually go out and approach other teams, but it's been
a bit difficult for us to find the time recently.  This is definitely an ambition though.

Looping back to the needs of the organisation, what we've tended to end up doing is use the enablers
as more of an infrastructure SWAT team, swooping in to unblock something that is stuck because of a
capability or personnel gap, rather than primarily being there as facilitators.

We haven't solved this problem yet, but we're working on it. As I'll discuss in a bit, we just need
to keep talking to the customers and making sure we're doing what we're here for, which is as
enablers for streamlining delivery. What it has underlined though is that we still have a way to go
to reduce the product teams' cognitive load to allow them to concentrate on their products.

### Sweet, sweet dogfood

The biggest issue we encountered as we started to have to maintain the shared modules, namespaces,
permissions etc. was that, in trying to run too fast, we'd deferred loads of stuff like automation.
We'd set up a mechanism for components to deploy stuff through our CD pipeline, but we hadn't
actually got round to using it ourselves. The consequences of this were twofold.

Firstly, every time someone encountered a problem, we had to relearn how it was all set up before we
could help. This was frustrating and slow for us, and also frustrating for the developers as we
weren't even eating our own dogfood. It's difficult sometimes to demonstrate empathy if you're also
demonstrating a 'do what I do, not as I say' practice. Now we're pretty good at this with things
like our observability stack, security tooling, infrastructure as code, etc. It was easy to
sacrifice the time for some of the automation though as we could 'just live with it'. Spoiler alert:
You can't for very long.

Secondly, and most acutely for us, as we expanded the amount of environments we had deployed, the
number of instances of terraform modules we had to look after grew exponentially. As I say, up until
now, we didn't have any CD for applying these, we were doing it manually. So every time a team
needed a quota change, or worse, a new feature or IAM permission, we'd have to go and version bump
something like thirty instances manually. This was not a good use of anyone's time. We knew this,
but the rate of change from the org, meant that it was very difficult for me to carve out the huge
chunk of time it was going to take to unravel this mess.

We'd fallen into the classic trap of identifying a huge, ugly overbearing problem and trying to fix
it in one go...

- we didn't spend the time automating our own stuff which left us with tech debt and toil
- we didn't then have the time to adopt and dogfood a lot of our own stuff, so we didn't see the
  issues

## Lessons learnt

### Reduce Toil

Make time for automation. Anything you automate is time freed up for more creative work. This is so
important for your engineers for one as toil can be soul destroying, but ultimately it starts to
impact on your ability to achieve your primary mission, to simplify things for your customers.

If it takes days for you to spin up an environment for a team, that's not a great experience for you
or them.

This might be unbearably obvious, but during the heat of sprinting to deliver something,
sometimes things just aren't as clear as they are in normal times.

### More dogfood

Anything you can dogfood, you should. From CD pipelines, to processes, to services. This is
important both for developing empathy with your users, but also so you have a chance to find and fix
issues before your users. No amount of unit tests is ever going to be a substitute for user
experience.

### Product Management

The State of DevOps report is out this year from PuppetLabs, and if you've read it you'll notice a
familiar trend from last year, and that's a focus on internal platform teams as enablers. This year
it also goes into those high achieving organisations adopting product management techniques and
treating that platform as a product. I've been half-arsing this for a while now, but in the last few
weeks, we've started to earnestly define our platform better, and start to really incorporate the
feedback loop between us and our customer into what we do.

Identify who your customers are, know exactly what it is you deliver to them, and share a mutual
understanding of that. We did some introspection on this recently, and the team said they felt like
they didn't know what the bounds of what we owned were, and that we were a catch-all team for all
the jobs other people didn't want to do. I have to say after twenty years doing this sort of work,
it's a recurring theme. Your internal product might be way bigger and fuzzier than, say a component
team in a microservice architecture's, but you should still be able to define it. We've talked about
reducing cognitive load for developers, and this is what we're here for, but we also have to think
about our own cognitive load.

Buy what you don't need to build. The likelihood is that your infrastructure needs ain't that
unique. Unless you're a cloud vendor, your infrastructure is not likely to be the key differentiator
and USP of your organisation. That doesn't mean it's not foundational to the success though. It just
means that boring solutions are often the best ones. There are still thousands of opportunities to
be creative, without running a bare-metal ElasticSearch cluster.

Now this is an innocuous sentence, but this is an easy thing to say, but a difficult thing to
define. For more info, search for ContainerSolutions WTFInars on Platform as a Product with Matthew
Skelton for some really great insight and discussion.

### More training and workshops

Documentation is great, it's a really good reference tool, but unless you have tecnical writers
available, it's not worth your time to write an all-encompassing narrative guide to your platform
and keep it up to date.  Have enough in there that you can use it as a comprehensive reference, And
try to automate it where possible. We use a bunch of mkdocs sites for each discrete chucnk of
infrastructure that sits in the repo with the code. It then all gets stitched together by CI into a
single cohesive site. There are things in there like AWS accounts and that sort of thing that then
get pulled in dynamically on build. We've also built a tool to generate AWS IAM configs and kube
configs based on user names for people.

Docs are imperative but training helps much better for onboarding. How you do that is really going
to depend on your own situation, but we've found value out of a live coding exercise, for example,
instrumenting someone's component with the prometheus library then building a grafana dashboard. You
can do a very basic version of that in an hour. Now, this does take time, both to prepare the
material, and to get enough people's time at the right time to deliver it, so only you will know
whether that will work for your organisation.

Think about enabling teams. We've found that they really enhance our platform team by having the
freedom to go and spend say three or five sprints inside another team, helping them to solve a
specific problem, and leaving them with the capabilities to understand and run with it. Again, we've
had mixed results from this, but I will say that the feedback from every engagement we've had over
the last 18 months has been overwhelmingly positive.

### Shift left, but not too left

Think very deeply, and talk to the dev teams, about exactly what capabilities they need inside their
team to do what they need to do. We are clear as an organisation about the fact that component teams
develop, own, and run their products. In order to do that, we need cross-functional teams. WHen you
adopt this approach, your product is no longer just your code, it's code, config, environments,
deployments, monitoring, logging, etc. Think deeply though about how much they need to know about
each of these things. Do they need to have a high-level overview of how things hang together in
Kubernetes? Probably, yes. Do they have to understand the Calico CNI though? Absolutely not. Again,
this might be a grey area, and it will differ in your organisation, so do the work to find out. I
can get in my car and drive to Sainsbury's without knowing how to replace the oil filter, because I
know I can take my car to a mechanic to have her fix it for me. It doesn't change my ability to get
last minute snacks for the Lions match on Saturday.

## Conclusion

When we started to really look at how to do devops, we, like I imagine many people, concentrated a
bit too much on the process, tools and engineering. The real value is in your relationships and
interactions with other teams. Knowing what kinds of team archetype you have from Team Topologies is
a good way of seeing what kinds of interactions work well. It can't tell you everything, and it
can't magically fix your culture, but it can make you start to think about things a bit more
critically.

As a platform team, it's very easy to hide behind the X-as-a-service interface and accidentally
become siloed. Set up these feedback mechanisms. Keep talking to each other,
and never assume you know what's best. Honestly, get a Product Manager. You may think you can do all
these things with whoever is currently in your team, but you really can't. Get someone in there that
really understands product management to help you develop your platform as a product.


## We're Hiring!
