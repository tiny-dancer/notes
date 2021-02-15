# Origin Story

## How it began (we're on the right path)

- The year was 2018, talk about public cloud being a reality, we had the benefit of learning from the prior 10 years and other fortune 500 adopters.  Starting off right with highly mature CI/CD practices with everything configured as code.  Following the success of doing this in another team, we went all in on Jenkins with highly sophisticated pipeline files with a custom YAML configuration layer.

## What happened (we can see the road but can't find it)

We did all the things expected of a highly mature CI/CD team and put all our marbles in the delivery pipeline.  We had 100% fully automated deployments derived from a single artifact for each environment.  Changes were validated in a pull request pipeline in a live environment specific to the pull request.

We could barely make progress.  

Our code was so intertwined with our pipeline that we were spending more time writing groovy code (jenkins pipeline) to add new features than terraform code.  On top of that, testing a change would take ~30 minutes to wait for a pull request pipeline commit to finish its verification.  As our deployment logic was so heavily tied to our pipeline, it we leveraged the pull request pipeline to test in flight changes - each verification of a change took 30-45 minutes.

We were drowning in our pipeline, the very thing meant to enable us to move fast and innovate.  

## A Fresh Look

Appreciating we were building the foundation of a fortunate 6 healthcare company's public cloud presence that will last for many years to come.  We approached our delivery needs with a fresh look.  The resulting mission was to rewire our practices to create a development environment where:

- No pipeline code necessary to add new features
- Changes can be easily and reliably tested from a local environment

## Runiac

Runiac was born to improve the experience developing and deploying infrastructure as code.

We like to think of it as what expo did for react native, runiac is doing for terraform, arm templates, and other iac.

### No pipeline code necessary

Runiac solves this in two ways.

1) **Runners**:  Runiac executes the specific iac tool on behalf of the caller,  this enables every execution of every project of every supported tool to use the same `runiac deploy` command. 
2) **Steps**:  Runiac features a folder convention to declaratively define how iac is executed.   This includes the ability to have runiac execute across multiple regions or execute workflows.

### Changes are easily and reliably test changes from a local environment

- **Containerized**: Runiac ensures reliability between between any environment the project is executed, including local, by executing the *steps* and *runners* in a container.  

Between *runners*, *steps* and being *containerized*.  Developers can now use the same `runiac deploy` command as the pipeline to iterate on changes locally without worrying if they are missing any specific pipeline magic in their local flows.


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4MTU2MDc3MDYsLTk5OTUxMTY3OF19
-->