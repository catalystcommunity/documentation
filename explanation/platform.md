# Catalyst Squad Platform Concepts

Distributed systems often get described as being difficult to think about. We believe there are easy analogies that explain just about everything one needs to know about distributed systems. In our case, we believe the most comprehensive one is a grocery store.

Good systems are about API contracts and separation of concerns. A monolith that is well organized is no exception, and distributed systems of all kinds simply require good citizenship to be utilized.

When you go to the grocery store, you don't need to know what their setup is like. You know there will be shelves, that prices will likely be posted, and that you put things in your cart. You take those to the front, scan them all, and pay the total. Bagging and loading takes place.

Your API contract is with the grocery store. You have lots of interactions with the grocery store in various small subsystem ways. Pricing, store layout, etc.

Where does your milk come from? Where does this box of granola bars get made? You typically don't care. That's not your concern. You care about the classification of some of these things. Maybe you want free range eggs, but which farm specifically is not often the concern.

This is the basis of the Catalyst Squad platform philosophy. Care about the grocery store, not the logistics behind it. If the grocery store needs to be replaced, you just have to find a new implementation of its API with minor changes. Change the URL and you'll be back in business.

This is cloud native thinking, and if you are building a grocery store, or a dairy farm, or a food shipping service, this is important. Let's talk about a few concepts that are important thinking in a Catalyst Squad Platform

## Metrics > Logs > Shelling

Record everything, but you want metrics first, logs second, and shell access never. Shell access implies a lot of things that are not good, but most importantly any time you need shell access outside your local dev environment, you should consider it a break glass event and do a post mortem.

Logs are good for capturing events produced, and analyzing them. They should in any case you are capable be produced as JSON, with at least the fields `service`, `loglevel`, `message` and if it is in a web request chain, `request-id` should be included.

Logs are sadly only good for debugging specific events, and they are often needles-in-haystacks scenarios. If you want a good picture of health in a system, you want to capture metrics. Metrics of everything you can. Number of requests, received bytes, request time, query time, anything you can count or set a gauge of.

Metrics give you the ability to create dashboards and at a glance see the health of anything you can predict. When troubleshooting, they give you something you can correlate with other metrics, even on the fly.

Metrics also put themselves in a very important scope of change over time. Often with logs one sees a log and doesn't know if that's new without looking through a lot of logs. With metrics you're already looking at the graph over time or the value at specific intervals at the very least.

So prefer metrics.

## We are Not Special

When we discuss a practice or tool or technique with our colleagues in the industry, two common arguments emerge, though of course never together.

- [Big Company X] does it this other way
- We are not [Big Company X]

This is a good sign that the discussion is going to go nowhere. We have a better idea instead. What a given company is or is not doing is not helpful. What the industry is finding success in or what the community is leaning on is good signal to assess.

This does not mean blindly follow the popular trends. Far from it. What this ultimately means is that there are well trodden paths. There are always several. There are always tradeoffs.

Do not blaze your own path if it is not your organizations core value add. You do not want to invent new paths. The more new paths you invent, the more you risk finding pitfalls. New paths are slow in all cases because it is new engineering. Make sure there is a return on that investment of time.

What the mantra of "we are not special" means is that if there is a database already available, you will very likely fail to invent a better one. If there is a library for logging, you should absolutely not invent your own.

Often the rebuttal to this is that "well, we need X feature." This is where the mantra becomes powerful. Chances are extremely good that you do not, in fact, need X feature. Challenge your assumptions. A good question to ask in these cases is, "why does nobody else seem to need X feature then?" Sometimes this is a form of YAGNI, but the thought exercise is more important, because the problem is not whether or not X feature is needed, the problem is why we believed it was needed in the first place? We should dig deeper always.

In a Catalyst Squad Platform, we try to use well worn paths to give a good foundation to build software. We avoid special cases and instead lean on well researched and tested tools and techniques to achieve organization outcomes. There are other tools and techniques that could work as well, but our choices have been made to work well together and keep special one-offs to a minimum.

## Separation of Concerns

In your kitchen, you have a set of tools. Almost everyone has a couple of tools that have only one purpose. Imagine a garlic press. It is only useful for garlic. If you need to press garlic often, this might be awesome, but the garlic press design is good for a specific thing and not others.

This is not what separation of concerns means. It could be applicable to separation of concerns. If you invented a garlic press because you press garlic once a month, you have failed as an engineer. If you invented the same thing because you're doing it 20 times a day, great success. The garlic press is deeply coupled to the garlic and the human. Maybe that fits requirements, maybe it doesn't.

If you need to invent a knife, however, starting with a serrated edge is silly. Where did that requirement come from?

More practically, the Catalyst Squad Platform is built to make sure that things care as little as possible about anything. A service doing X shouldn't know if it's in nonprod or prod, it shouldn't know if it's in a docker container or in an IDE debugger, and it shouldn't know if it's talking to a database or a mock. It doesn't own any of those things, so it shouldn't care.

That's what separation of concerns means, caring about what needs to be cared about, and nothing more.

What this gives us is extremely flexible and replacable lego bricks to build with, and strong API contracts to build to. If your code speaks grpc, it doesn't have to know we replaced the python service here with a rust service there. It just knows the URL and protocol it needs to speak to.

Care about less, do it in more places, compose structures we need out of those. It will feel like we have to learn more things, but really it's just a lot of smaller things that are more consumable to our minds as opposed to a huge thing that is less of a bullet list but much more of a meal to chew on.

## Some technical notes

We prefer to work in Go and Rust. They're efficient to run, and enforce good engineering habits for a long term investment. In a view of a decade, these are not a huge learning curve.

We prefer to run workloads in kubernetes. It's very flexible about letting us separate concerns and compose the right behaviors. It's the right abstraction layer for engineers to own their services and work together at human scales.

We prefer github actions for CI/CD, though we can implement the CI/CD philosophy we have in most tooling. We have standards about this (see [We are Not Special](#we-are-not-special))

In every case we prefer an open source, self-hosted tool or the equivalent cloud hosted version if competently run by the vendor. There's a few exceptions now and then, but in general this increases ownership of teams, reliability of services, and makes longevity of products more probable. Nevertheless, the tradeoffs must be explored, always.

We prefer tools that have opinions about workflow that have been proven to work. Angular is a good example of an opinionated framework. We wish to conform to these opinions knowing they are about things we do not need to do. We want well blazed trails where possible. Golang is an entire opinionated language. We often disagree with these opinions of these tools, but it is worth conforming for the goals of the product being built to set our ego aside and evaluating.

We prefer to avoid "Classes" as in OOP classes. What Java and C# we write you will note is very straight forward and not very OOP. You do not have to agree, but if you with to PR to our libraries or services, you will need to know our style to fit it.

## A note on versions

Some of our workflows are sophisticated about versioning. Some of our products are also sophisticated about versioning. Some are not. This documentation repo for instance has no versioning at the time of this writing. It will improve over time. We are not purists about much, and will use versions where it adds value to an org, but not just because it's a Good Thing™ as an axiom.
