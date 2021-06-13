
# Value Delivery [wip]

## The 'why' of value delivery

### Why software engineering exists

- the history of how it became and explanation of it's sole purpose: delivering value to humans

### The 'how' of value delivery

-    **Synergy:** - Creates conventions to build in consistency.  Consistency enables cohesion where a system can automatically leverage other parts so that the sum is greater than the whole.  For example, creating an api where then a pagerduty service, oauth2 client, and baseline alerting is automatically created to reflect it's existence.

-   **Think large, start small (agile)** - Optimize for reacting to learnings over predicting the future.  Define a true north and evolve towards it with flexibility based on evidence.

-   **Ephemeral Environments:** component changes are reflected in a safe, isolated environment mirroring a production deployment and integrated with existing environments for valuable testing and deployment validation
    
-    **Portable Execution:** the deployment logic is not dependent on being executed within a specific pipeline; this enables valuable, quick-iterating local development and future ci/cd system changes
    
-   **Smart endpoints and dumb pipelines**: pipelines are workflow orchestrators only executing source controlled, portably executable deployment logic that is not dependent being in the pipeline (terraform, arm, bash, etc)
    
-   **Feature Management (Toggles)**: Any new changes are wrapped in a feature toggle allowing the codebase to be safely deployed to production at any given time. This also allows for releasing features safely to a subset of users.
    
-    **Everything as Code:** Self explanatory, everything that can be configured as code eventually is. This greatly minimizes toil time and once implemented helps maximize developer day to day efficiency. Enables repeatable and consistent deployments with no configuration drift (ephemeral environments, high release/deployment rates)
    
-   **Culture and Habits**
    
    -   **Developers Shepherd Changes:** a developer owns the change they are working on and becomes its "release advocate". They incorporate others as part of the development cycle but it never "becomes someone else's problem" to ensure the change is safely released to production.  This habit surfaces when a release to production can occur within days or hours after "dev complete" and helps drive home the paradigm that "value is not realized until users experience the change".

    -   **Team Enablement (Organizational/Human):** a team is empowered to own their application and control their destiny. In cases where shared services and platforms exist, they must enable above paradigms such as everything as code

    -   **App Enablement (Pipeline/Machine):** an application/component deployment pipeline is empowered to control its own destiny. In cases where shared services and platforms exist, they must enable above paradigms such as everything as code and api-driven, programmatic interactions

High level keys:
- Getting to a point where production deployments are so safe and repetitive they became boring daily events
	- This foundation event is made possible a lot of intricate delivery practices and planning
- There is no single silver bullet process for rolling out changes safely on such a high cadence however it is common to develop a standardized set of approaches.  During planning/grooming each feature would be tagged to one of these approaches depending on risk size, data schema changes, external systems, etc.

There is one constant in digital & tech development: things will change. This is why agile exists, to embrace the change and move with it. This means the most important part of any digital system is the part no user sees: the ability for the system to support change. This is the flow of value going from idea to user: delivering enhancements to the digital portal, we’ll call it the “Delivery Pipeline”. Too often we see in consultancy engagements (or any technology project) the focus is placed on the digital portal seen by the end users with a lack of focus on the delivery pipeline. At the end of the engagement there is a working digital portal however every subsequent change or enhancement to the digital portal has a time to deliver multiplier hindering the business to respond to the market and users. With timelines multiplying and deadlines missed, it’s too common to then re-engage with the consultancy firm to add new features for end users, where again there is a lack of focus on improving the “Delivery Pipeline” and the issue circulates.

It's a similar experience with cars. We often purchase cars based on looks (the digital portal) however stay loyal based on what people don’t directly see: the engine and overall reliability. We want to help ensure your car’s engine is well prepped and ready for you to enjoy a long road ahead you, avoiding the headaches of breakdowns and tow trucks.

Too many times we’ve seen technology hamstring the potential of the business and after helping correct the course it’s very disheartening to consider the opportunity loss if the initial direction was a little different from the beginning.

#### Delivery Framework: Flow of Value

Delivery Framework and it's codified implementation: CI/CD 

- ##### Less changes, less risk

The key paradigms enabling the flow of value is:

##### Ephemeral Environments

> Imagine you could safely test your changes directly in production separate from end user traffic, would you really need another environment?

The ability to spin up an isolated, production-like component within an existing environment

--> Add Quote from MS CIO book

##### Why It Matters

When realizing value the only environment(s) that count are the ones where live end users interact with the system.  We will call this production.  In a perfect  world with no mistakes or defects there would only be production and everything would be deployed directly there.  With this appreciation and knowing we unfortunately do not live in an everything is awesome world, we are required to layer in "non production" environments to first verify the changes work as expected.  The common practice here is to start with a 4 environment promotion path: "Dev" --> "Test" --> "Stage" --> "Production", this is a mistake.  In the current state of our world, starting with this approach is a mistake.  Remember, when realizing value the only environment that matters is Production and environment before production increases the amount of time and effort it will take to deliver value to to production, therefore lowering the speed of delivery rate.  It is important to start flipping this around and starting with the least amount of environments required and then adding environments as needed.  Taking this a step further, we can introduce temporary, "ephemeral", environments to replace the time investment of a persistent environment.

- An environment requires care and feeding
- Changes tend to accumulate in each environment and by the time changes make production, they include far more than a single change which introduces higher risk, higher deployment time, and more people required to validate the changes.  
- Ties into pull request canary releases and safely pushing left the validation of potential changes in production

Examples:

- Web Application
	- Netlify deploy previews are a good example of this.  If production lives at https://app.awesome.com.  An ephemeral environment matching production could be available at https://pr-123-app.awesome.com
		- https://docs.netlify.com/site-deploys/overview/#deploy-preview-controls
- API
	- If a production api is available at https://api.awesome.com/cool, the ephemeral environment may be available at https://api.awesome.com/pr-123-cool 
- Infrastructure

Future posts: Handling seed data and db migrations.  Ask for comments: how have others solved these problems?

##### Portable Execution

The ability to execute a deployment consistently from any environment

Example: 

- Utilizing a docker container to execute a deployment.

#####  Smart endpoints and dumb pipes

In continuous delivery this is the practice of maintaining a simple pipeline ("dumb pipe") which delegates the deployment logic to a portable execution component ("smart endpoint"). In this model the pipeline defines the workflow but is not responsible for the 

##### Why It Matters

- When teams adopt Git Ops and Continuous Delivery it's a common practice to ensure everything is automated in the Pipelines.  Tools such as Jenkins (pipeline as code) and Azure DevOps (pipeline templates) make this easy to do.  It is important to ensure the pipeline does not become coupled to the deployed application.  This introduces another system to modify and test along with being a risk point for future and existing deployments.  Additionally it eliminates the ability to reliably test an environment changes prior to a pipeline execution

- Another system to modify and test
- A risk point for the next deployment and potentially existing deployments (e.g. breaking the ability to perform a rollback)
- Eliminates the ability to reliably test an environment prior to a pipeline deployment
	- Developer feedback on change. 1m to 5m. 

- Add examples such as using pipeline templates to represent infrastructure components

- https://martinfowler.com/articles/microservices.html#SmartEndpointsAndDumbPipes

##### Feature Management (Toggles)

> Software developers need a new set of tools that are specifically designed for a world with continuous delivery. What if you could fully decouple the act of delivering software from the act of releasing features? What if you could deploy any time you want but release only when you’re ready?

Said best by John Kodumal in _Effective Feature Management_, Feature management was born to solve these problems.

##### Why It Matters

Proper feature management enables a code base to always be ready for a production deployment.  The ability to accomplish this greatly simplifies the amount of process and time required to manage work in progress, allowing all changes to be treated to same regardless of urgency and timelines.

--> Branching strategies

##### Everything as Code

This is the practice of configuring services in automatable code rather than manual click through visual experiences. Everything as Code is the key ingredient enabling teams to adopt a highly mature delivery framework in a short amount of time.  Everything as Code leads to the benefit of "solving a problem one time while benefiting N times"; in the open source world this benefit can span across entire industries.

Examples

- Terraform (Modules)
- Azure YML Pipeline (Templates)
- Jenkins Pipeline as Code (Libraries)

#####  Culture and Habits

- **Value shepherds**:  The developing engineer will actively work to ensure their change makes it successfully into production. This shifts the focus of the developing engineer from "completing the necessary code changes" to "ensure the new change is successfully in production".  After completing the necessary code changes, the developing engineer will be a personal advocate and supportee to the change for ensuring subsequent tasks are completed efficiently.  If any unexpected issues arise during testing of the change, the developing engineer is the first to respond and resolve issues.  If they find the change is stuck in limo somewhere, they are the first to raise awareness and ask for progress.  They ensure their change benefits users as safely and quickly as possible. 

##### Why It Matters

This is the most important item for 2 primary reasons.  

1: Human compassion, improving oneself and those around us is far more important than accomplishing the goals of a non-sentient company.

2: one could engineer the most efficient delivery system possible, but if they don't know how to utilize it nothing will change.  Even more likely, the delivery system will digress as people fall back to what is more comfortable.

#####  Team Enablement (Organizational)

Ensuring the teams delivering the products are enabled to control their own destiny.

--> Talk about ownership (one back to pat), cascading issues between interdependent teams, etc. (devops)

For example, if team a depends on team b 

#####  App Enablement (Pipeline)

Ensuring an application is enabled to control their own destiny.

For example,  if a web application will be made available at awesome.com, allow the pipeline for the application to manage the awesome.com configuration.  This is primarily enabled through cloud first api-driven automation and proper permissions to ensure the application pipeline could not accidentally, or maliciously, modify fun.com,

Other examples, include enabling API's to self register in an API Gateway through their pipeline.  

The key is ensuring when an application's pipeline is executed everything that needs to be configured for it to work is configured.  

##### Why It Matters

> *Everything is Connected:  This enables `Ephemeral Environments`*

##### Methodologies

- Branching Strategy
- Feature Toggles
- Team Delivery Culture and Practices

##### Tools
		
- Pull Request Based Development
- Azure Pipeline Templates
- Terrascale (terraform value delivery framework)


#### Product: The Value flowing through the Delivery Framework

- Infrastructure architecture
- Application architecture
- Site Reliability
- Security
- Reusability
- Component Libraries

#### Value Pillars

- User Trust
	- Site Reliability
	- CI/CD
	- Security
- *parallels to user trust?*

#### Topics

- Additional considerations


## The 'what' of value delivery

### Branching Strategy

- github flow, pull request based development

- Azure Pipeline Templates
- Terrascale (terraform value delivery framework)
- Terraform

- https://humanitec.com/blog/ephemeral-environments-for-testing


# Notes

## ProtectWell First Steps

### Improving user trust in the application

-   Self Governing ProtectWell Application
	-   We know immediately when things go wrong
	-   Automated incident management, escalation and notifications
	-   Continuously running VBF automation suite in production

-   Buy vs Build: Minimizing custom developed software for provided alternatives
	- Cloud Native
	- Open Source
	- Third Party
-   Reduces the amount of failure points owned by Application
-   Security: Security scans and remediations

First item(s):

-   Monitoring and Alerting
	-   % of successful log-ins
	-   % of successful symptom checks
-   PagerDuty: when a service and VBF encounters health issues Pagerduty is notified

-   PagerDuty than notifies the appropriate on-call engineers and escalates to more the longer the issues remain open
-   Will also automatically notify the Production Incidents teams channel

 
**Improve product agility and engineering culture**

-   Impacts _everything,_ enables _everything_
-   Shifting from risky, manual, slow 2 week release cycles to safe, quick, on-demand releases
-   When things go wrong we can react, resolve and deploy in minutes and hours rather than days.

First Item(s):

-   New mobile api created specifically for handling mobile traffic
-   This allows us to safely scale, configure and release most of our mobile system without impact to or from admin console requirements and features.
-   This api is fully automated and built for on-demand releases
-   Automated testing built into each deployment
-   This mobile api is built as a re-usable template and every other part of the system will mature into these standards quickly

# References

- https://www.derekashmore.com/2016/03/book-review-art-of-business-value-by.html
- https://www.amazon.com/dp/1942788045/ref=rdr_ext_sb_ti_hist_1
- https://www.amazon.com/Continuous-Delivery-Deployment-Automation-Addison-Wesley/dp/0321601912
- https://www.amazon.com/Site-Reliability-Engineering-Production-Systems/dp/149192912X
- https://www.amazon.com/Unicorn-Project-Developers-Disruption-Thriving-ebook/dp/B07QT9QR41
- [DevOps Paradox: The truth about DevOps by the people on the front line](https://www.amazon.com/dp/1789133637)

TOREAD:
- https://www.amazon.com/Seeking-SRE-Conversations-Running-Production/dp/1491978864
- https://www.amazon.com/Pragmatic-Programmer-Anniversary-Journey-Mastery/dp/B0833FBNHV
- https://www.amazon.com/Ends-Game-Companies-Delivering-Management-ebook/dp/B084V7TRRF
- https://www.amazon.com/21st-Century-Corporate-Citizenship-Delivering-ebook/dp/B06VT5FH7G
- https://www.amazon.com/Real-Business-Create-Communicate-Value/dp/1422147614/
- https://www.amazon.com/Business-Analysis-Agility-Problem-Deliver-ebook/dp/B07J2PMFL7
- https://www.amazon.com/Systemic-Coaching-Delivering-Beyond-Individual-ebook/dp/B0829C831X
- https://www.amazon.com/Pass-Transferring-Wealth-Financial-Generations-ebook/dp/B08KBNBWX6


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1MzE4MDc1MjUsNTY4NjczNTM0LDE0Nj
gyMDAzOTIsODI5ODk4NTEyLDY0MzcwNDUxMSwxNTE4NzExMDg2
LC0yMDEyNzg1MDI1LC0xODU1ODAxNTUwLDExNDk1NTczODYsMj
EwMjExMjE2MCwxOTM1NzA4MzE3LDIxMzI1NTgyNjcsNDgwMzc2
NTEzLDMyODM4ODY3NSwtMTUwNzYzMDIxNiwxNDE0OTUyNjIzLD
gyMjE4MzkxMSwxOTg1MjUwOTgsNzEwMDY0MjY2LC0xMjAxODc2
MTAyXX0=
-->
