# Origin Story

TL/DR

- Building out public cloud platform for fortune 6 health care company.
- We thought we were awesome codifying everything with terraform and running it through jenkins with pipeline as code.
- We were not awesome, making changes and improving things took forever as we had to use and add to the pipeline for each change. 
- New mission to *Focus on value code [not pipeline code]* and *Faster feedback on changes*
- Adapt to enable *No pipeline code* and _local development with prod-like environments_
- Runiac born
- 220% productivity boost
- :rocket:

## The Challenge

> Building out the public cloud platform for UnitedHealth Group to sustainably adopt the public cloud safely while enabling developers and products to innovate.

UnitedHealth Group is a fortune 6 company with over 30,000 technologists.  Looking at the future public cloud presence for a company of this size was an exciting and daunting task.  We were uniquely positioned as a product engineering organization where customer value and friction-less user experiences are of the utmost importance.  We wanted to ensure public cloud maintained the team's benefit of adopting it to control their own destiny, while at the same time having a paved path of secure, expedited cloud growth.

To accomplish this mission, we knew we would need to have a cloud platform capable of securing and enabling 3,000 cloud accounts across each major cloud provider.  1,000 gcp projects, 1,000 aws accounts, and 1,000 azure subscriptions.

## The Beginning: On the right path

The year was 2018 and talk about enterprise public cloud adoption was shifting to a reality.  Starting at this time we had the benefit of learning from the prior 10 years of other fortune 500 company's cloud journeys.  This led to public cloud principals of everything as code, friction-less developer experience and no human changes in production.

I and others starting the new public cloud platform engineering team had recently come from building a highly performant developer platform.  We knew how to deploy and run software fast, safe and at scale where one or more production deployments a day is part of the normal operating procedures. 

We were confident and excited to bring this same culture to a public cloud infrastructure platform, ignorantly believing the hardest was behind us in true leroy jenkins fashion.

## What Happened: See the road but can't find it

We did all the things expected of a highly mature platform engineering team.   Everything as code was the mantra and terraform with jenkins would be our ticket to paradise.  

We kicked everything off 'right' with highly mature CI/CD practices with everything configured as code.  Following the success of doing this in another team, we went all in on Jenkins with highly sophisticated groovy pipelines with a custom YAML configuration interface.

In short time we had fully automated deployments progressing through a single continuous delivery pipeline deploying the same terraform code with different variables to each environment.  Pull requests were deploying changes to an ephemeral environment specific to the pull request ensuring every merged pull request succesfully deployed a living environment.  As part of this pull request pipeline we would go as far as pre-baking the ephemeral environment to match production, therefore each merged pull request was proven to be successful in deploying to an environment matching prod.

We could barely make progress.  

Our code was so intertwined with our pipeline that we were spending more time writing jenkins pipeline groovy code when adding new features than terraform code.  On top of that, testing a single change would take ~30-60 minutes to wait for a pull request commit to finish its pipeline verification.   Based on those numbers a developer working on a task would get no more than 5 chances to verify their work before hitting near to 3 hours of pipeline wait time.  3 hours of waiting each day.  Those who work with clouds and Terraform appreciate the power of guess and checking quick configuration changes, the faster we can validate changes the better.

I assure you stating this so concisely is a benefit of captain hindsight.  At the time there was a lot of effort swirling around improving the pipeline, delivering  features, learning the in's and out's of each cloud and many other things.  It took longer than one would like to admit in determining our "advanced" process was causing engineers to spend an just about the same amount of time developing as waiting.

Deploying the code from our local environments was not an easy task based on how heavily tied our deployment logic was to our pipeline, therefore it was labeled as a 'nice to have' and not a popular option.

We were drowning in our delivery pipeline, the very thing meant to enable us to move fast and innovate.  

## A Fresh Look: Paving a new road

Appreciating we were no longer deploying a typical application but uniquely developing a cloud platform with terraform as our primary language, we took a big step back and approached our development and delivery with a fresh perspective.    This fresh perspective led to two driving missions:

- *Focus on value code*:   Valuable developing time on this team was infrastructure code, _not pipeline code_.  A pipeline is a hugely important means to an end.  The pipeline itself is not the product, the product for an infrastructure platform team, is the infrastructure.    Our code base was roughly ~45% groovy,bash (jenkins) and 55% terraform (iac).  The churn between them was scaringly similar as well. Meaning we spent close to the same amount of time on the "means to end" pipeline as the valuable infrastructure code.  We needed to ensure we spent as much time as possible on infrastructure code.

> Pipelines are **hugely** important, if not the most important item, to the success of a product.  The key is ensuring the pipeline works for you over time and you don't work for the pipeline.  We were spending too much time on pipeline code, never receiving the time benefits of the pipeline doing the heavily lifting for us.

- *Faster feedback on changes*:  The faster a developer receives feedback on there change, the more changes they can test, the more changes they can test the more they can develop and with a high quality.   Waiting 30-60 minutes for feedback on if your change worked or not was a huge inhibitor to developing quality products efficiently.  One would 

> To clarify, A change in this context could be a simple as a single line change in the code.  Any individual code level change that is ready to be verified.   It is common to make hundreds of these micro change + test feedback loops a day when developing.

This led to two focus points for our updated process:
- **_No pipeline code_** necessary to add new features
- Changes can be easily and reliably tested from a **_local environment_**

We were wary of developing any custom tools and were hopeful to leverage existing.  Surprisingly, we did not find a solution that targeted these two items specifically.

> Terragrunt is a fantastic tool easing the use of terraform created by a fantastic organization, however it does specifically solve these higher level developer experience items and similar to terraform relies on implementing specific terragrunt deployment logic to run it

To solve the problem we decided to use two incredibly common tools:

1. Folders (yes, you read that correctly)
2. Containers (docker)

1) To avoid writing pipeline code, we would fully embrace the declarative spirit and declare order of options between terraform projects using folders.

2) To ensure the local and pipeline deployments were always 100% in-sync using the same deployment logic, we would leverage a docker container to execute our deployments and run terraform.

Enter...

## Runiac

We like to think of it as what expo did for react native and what webpack did for front end, runiac is doing for terraform, arm templates, and other iac.

One command, one workflow, any tool.  Currently supporting Terraform and arm templates

### No pipeline code necessary

Runiac solves this in two ways.

1) **Runners**:  Runiac executes the specific iac tool on behalf of the caller,  this enables every execution of every project of every supported tool to use the same `runiac deploy` command. 
2) **Steps**:  Runiac features a folder convention to declaratively define how iac is executed.   This includes the ability to have runiac execute across multiple regions or execute workflows.

### Changes are easily and reliably test changes from a local environment

- **Containerized**: Runiac ensures reliability between between any environment the project is executed, including local, by executing the *steps* and *runners* in a container.  

Between *runners*, *steps* and being *containerized*.  Developers can now use the same `runiac deploy` command as the pipeline to iterate on changes locally without worrying if they are missing any specific pipeline magic in their local flows.

## The Impact

Transitioning to runiac led to a 220% productivity boost in developer time for our public cloud platform engineering team adopting it. 
  
### Feedback Loop  

Time spent waiting on testing/validating a change made to the iac.
  
* Runiac: 3 minutes  
* Previous: ~35 minutes  

Assuming a code change every 15 minutes, % time spent developing vs waiting on tooling/process:  

* Runiac: 83% time spent writing valuable code  
* Previous: 30% time spent writing valuable code  
  
Impact: A developer using Runiac will spend 3.7 more hours each day developing valuable features (7 hour work day assumed)  
  
### Value of Time (Focus on value code)
  
- Runiac: 95% of the code is valuable cloud code in repository.  
- Previous: 66% of the code is valuable cloud code. 33% is pipeline code.  
	- Roughly 33% of development time for _every feature_ is working on non-product value add code (pipeline code)  
  
Impact: A developer using Runiac will spend 95% of coding time directly on valuable terraform code, a productivity increase of 29% from previous processes
  
 ### Combining the two

Runiac enabled a 220% productivity boost. A developer using runiac over the legacy pipeline will spend 4.7 more hours/day on value add product code. Meaning in a team six, 5 days developing with runiac accomplishes the equivalent of 11 days using legacy, ~220%.

## Overall Impact

We hypothesized this productivity boost would translate to accomplishing road map items faster, more efficiency in completing work meaning work being completed faster.  Interestingly, the rate of features being completed remained the same.  We instead saw a 220% boost in overall quality and system reliability.  This was an interesting observation and is likely an excellent example of a psychological phenomenon, if you know what that would be please add a comment!

The end result: the successful creation of a world-class global cloud platform with sustainable adoption across 3 major clouds for a fortunate 6 health care company with 5 engineers.

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4NTcwNzMwMDgsLTE1MDE1MTEwOTQsOD
k1NjgyNDY4LC00MjE0Nzc3ODcsMTM0OTA4NzMxMiwtNzcwNDk5
NTAwLC0xOTAzNjE5Nzc0LDQ0OTk3MTk1NSwtMTkzNzQ4MTAyMC
wtMTk3MjYyMzEwNSwtMTc0ODM3NzEzOCwtODYzNDgxMDA3LDQ4
MzI5MDY0MCw2NzY3OTEzNTMsNzMyODA5ODQyLC03Nzc3MDMyMD
ksLTE4MDE3NTMwMDMsLTk5OTUxMTY3OF19
-->