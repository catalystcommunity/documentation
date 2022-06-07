# Local development

It should be noted that "local" is a term that has lost some of its meaning in the cloud native world. We develop using the screen, keyboard, and mouse in front of us, though the compute might not be strictly on just the machine those are attached to. Keep this in mind.

## General principles

We prefer to extend "not caring" about things we don't need to into the tools of our craft. This means we do not dictate editors, languages, runtimes, oses, etc. We instead have patterns and requirements and let those eliminate themselves. For instance, our preference to not care about OS means we do not lean on Windows software, though if someone used software in Windows, great. Our processes simply won't be coupled to it. Our standards for speed and avoiding overengineering mean we don't write Java, though we might use a Java program just the same.

We prefer not to rely on cloud services for development progress. If we can't develop something on our own machine because some cloud service is down, this is a bad time. Some exceptions for git or package hosting may be made, but even then chances are we could continue working on something without them as we'll have local cache for in-flight projects. Hosted services are still fine, but they should not be so important as to halt all progress.

We prefer workflows that take as little effort away from service ownership as possible. This means if a tool is used to setup a local environment, it should be as close as possible to non-local environments, and it should use similar tech. Configuration and composition should change instead of environment. The clearest example of this at the moment is we're very heavily Kubernetes oriented, so we develop in cloud native stacks. We try to avoid a docker compose stack as that would be maintaining something that goes in a different direction than the service we're developing.

We prefer engineering service ownership. We do not subscribe to the old ways of a different operations team running the service. Extensive research has been done in the industry on this subject. Accountability is required for quality, and that means engineering teams own the entire lifecycle of a service from dev to production to incident management. Teams can assist, tools may automate, but the result must be answered for by the owning team.

We prefer an [Inner Source](https://en.wikipedia.org/wiki/Inner_source) style model. The team owns a service and all the repos that contribute to it, but everyone in the org can see that source and make pull requests against it to add features or fix bugs. The team can still reject that pull request, or require changes or standards be met before approving such a pull request. At some point, humans work together, or no model will be successful.

We prefer automation where reasonable, not where it causes friction. A CLI tool that reformats according to style guides is reasonable, unless the configuration is broken. A pull request linter is reasonable. The guiding principle is that it should add value, reduce human friction, and be quick. A 6 minute setup time is not likely reasonable for common tasks. It might be reasonable for occasional tasks. Situational awareness is important.

We prefer not to use GUIs for setting up anything that should be in code or in a script. This is not a point about IDEs, it's a point about tools used for packaging, configuration, testing, or deployment. If you would find it in a CI/CD pipeline, it must be a CLI tool, always. GUIs are for status viewing where they exist.
