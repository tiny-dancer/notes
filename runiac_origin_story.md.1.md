# Origin Story

TL/DR

- We thought we were awesome and codified everything and ran it through jenkins with pipeline as code.
- We were not awesome, making changes and improving things took forever as we had to use the pipeline to test each change. 

## The Challenge

Maintaining a continuously secure public cloud while enabling developers to deliver value to users.

UnitedHealth Group is a fortune 6 company with 40,000 technologists.  Looking at the future public cloud prescense of a company this size was an exciting, while daunting task.  We were also uniquely position as a product engineering organization where customer value and friction-less users experiences are of the utmost importance.  We wanted to ensure the public cloud maintained the benefit of the teams adopting it to control their own destiny, while at the same time having a paved path of secure, expedited cloud growth.

We knew we would need to have a platform to secure and enable 1,000 cloud accounts, in each major cloud provider.  1,000 gcp projects, 1,000 aws accounts, and 1,000 azure subscriptions.

## How it began (we're on the right path)

The year was 2018, talk about public cloud being a reality, we had the benefit of learning from the prior 10 years and other fortune 500 adopters.  Starting off right with highly mature CI/CD practices with everything configured as code.  Following the success of doing this in another team, we went all in on Jenkins with highly sophisticated pipeline files with a custom YAML configuration layer.

I and others starting the new public cloud platform engineering team had recently come from building a highly performant developer platform.  We knew how to deploy and run software fast, safe and at scale where one or more production deployments a day is part of the normal operating procedures. 

We were confident and excited to bring this same culture to a public cloud infrastructure platform, blindly thinking the hardest was behind us.

## What happened (we can see the road but can't find it)

We did all the things expected of a highly mature platform engineering team. Everything as code was the mantra and terraform with jenkins would be our ticket to paradise.  We dove head first re-creating 100% fully automated deployments progressing through a single continuous delivery pipeline deploying to the same terraform code with different variables to each environment.  Pull requests were deploying changes to an ephemeral environment specific to the pull request ensuring every merged pull request had a functioning deployment and living environment.  As part of this pull request pipeline we would go as far as pre-baking the PR environment to match production, therefore each merged pull request was proven to be successful in deploying to an environment matching prod.

We could barely make progress.  

Our code was so intertwined with our pipeline that we were spending more time writing groovy code (jenkins pipeline) adding new features than terraform code.  On top of that, testing a single change would take ~30-60 minutes to wait for a pull request pipeline commit to finish its verification.   Based on those numbers, a developer working on a work item during the would get no more than 5 chances to verify what they were working on before hitting near to 3 hours of pipeline wait time.  3 hours of waiting each day.  

I assure you retros

Deploying the code from our local environments was not an easy task based on how heavily tied our deployment logic was to our pipeline, therefore it was not a popular option.

We were drowning in our delivery pipeline, the very thing meant to enable us to move fast and innovate.  

## A Fresh Look

Appreciating we were building the foundation of a fortunate 6 healthcare company's public cloud presence that will last for many years to come.  We approached our delivery needs with a fresh look.  The resulting mission was to rewire our practices to create a development environment where:

- No pipeline code necessary to add new features
- Changes can be easily and reliably tested from a local environment

## Runiac

Runiac was born to improve the experience developing and deploying infrastructure as code.

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

Add impact notes here from last years review.
<!--stackedit_data:
eyJoaXN0b3J5IjpbOTA4MjczOTExLDczMjgwOTg0MiwtNzc3Nz
AzMjA5LC0xODAxNzUzMDAzLC05OTk1MTE2NzhdfQ==
-->