
# Value Delivery [wip]

## The 'why' of value delivery

### Why software engineering exists

- the history of how it became and explanation of it's sole purpose: delivering value to humans

### The 'how' of value delivery

There is one constant in digital & tech development: things will change. This is why agile exists, to embrace the change and move with it. This means the most important part of any digital system is the part no user sees: the ability for the system to support change. This is the flow of value going from idea to user: delivering enhancements to the digital portal, we’ll call it the “Delivery Pipeline”. Too often we see in consultancy engagements (or any technology project) the focus is placed on the digital portal seen by the end users with a lack of focus on the delivery pipeline. At the end of the engagement there is a working digital portal however every subsequent change or enhancement to the digital portal has a time to deliver multiplier hindering the business to respond to the market and users. With timelines multiplying and deadlines missed, it’s too common to then re-engage with the consultancy firm to add new features for end users, where again there is a lack of focus on improving the “Delivery Pipeline” and the issue circulates.

It's a similar experience with cars. We often purchase cars based on looks (the digital portal) however stay loyal based on what people don’t directly see: the engine and overall reliability. We want to help ensure your car’s engine is well prepped and ready for you to enjoy a long road ahead you, avoiding the headaches of breakdowns and tow trucks.

Too many times we’ve seen technology hamstring the potential of the business and after helping correct the course it’s very disheartening to consider the opportunity loss if the initial direction was a little different from the beginning.

#### Delivery Framework: Flow of Value

Delivery Framework and it's codified implementation: CI/CD 

The key paradigms enabling the flow of value is:

##### Ephemeral Environments

The ability to spin up an isolated, production-like component within an existing environment

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

##### Everything as Code

This is the practice of configuring services in automatable code rather than manual click through visual experiences. Everything as Code is the key ingredient enabling teams to adopt a highly mature delivery framework in a short amount of time.  Everything as Code leads to the benefit of "solving a problem one time while benefiting N times" a

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
eyJoaXN0b3J5IjpbLTExNTY3NzExNDUsMzI4Mzg4Njc1LC0xNT
A3NjMwMjE2LDE0MTQ5NTI2MjMsODIyMTgzOTExLDE5ODUyNTA5
OCw3MTAwNjQyNjYsLTEyMDE4NzYxMDIsLTIxOTgwOTU3OCwtNj
M5MTMyOTk5LC0xNjg4MDY2ODI2XX0=
-->