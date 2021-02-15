# Origin Story

## How it began (we're on the right path)

- The year was 2018, talk about public cloud being a reality, we had the benefit of learning from the prior 10 years and other fortune 500 adopters.  Starting off right with highly mature CI/CD practices with everything configured as code.  Following the success of doing this in another team, we went all in on Jenkins with highly sophisticated pipeline files with a custom YAML configuration layer.

## What happened (we can see the road but can't find it)

We did all the things expected of a highly mature CI/CD team and put all our marbles in the delivery pipeline.  We had 100% fully automated deployments derived from a single artifact for each environment.  Changes were validated in a pull request pipeline in a live environment specific to the pull request.

We could barely make progress.  

Our code was so intertwined with our pipeline that we were spending more time writing groovy code (jenkins pipeline) to add new features than terraform code.  On top of that, testing a change would take ~30 minutes to wait for a pull request pipeline commit to finish its verification.  As our deployment logic was so heavily tied to our pipeline, it we leveraged the pull request pipeline to test in flight changes - each verification of a change took 30-45 minutes.

We were drowning in our pipeline, the very thing meant to enable us to move fast and innovate.  

## A Fresh Look

Appreciating we were building the foundation of a fortunate 6 healthcare company's public cloud presence that will last for many years to come.  We approached our delivery needs with a fresh look.  The resulting mission was to rewire our development practices to create an environment where:

- No pipeline code needed [after initial creation]
- An easy to use, meaningful local development environment

## Runiac

From this mission, runiac was born.  
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTY1OTYxNTE4NCwtOTk5NTExNjc4XX0=
-->