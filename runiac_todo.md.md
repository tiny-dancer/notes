
RunIaC

-   ability to intercept and run commands in terminal interactively (like a break point)
-   if error response includes “to be managed via Terraform this resource needs to be imported into the State”
-   better document the containers (e.g. terraform being used, etc)
-   document how to resolve “how can i tear down an earlier step when a later step fails?”

-   —> run the explicit step

-   better document learned lessons from steps

-   terraform runner: prefer a single step where possible

-   reasons to expand:

-   When it doesn’t make sense

-   eliminate runiac_environment being required

-   cli and terraform runner

-   configurable delay between the retries
-   ability to run it outside of runiac

-   backend.tf interpolation
-   step to step variable naming convention

-   runiac short cut to break lease on azure storage account
-   ability to remove state locking locally
-   document deploy configuration

-   runiac.yml
-   cli

  

Blog

-   utilizing a data/resource toggle to reuse persistent environment resources in ephemeral environments
-   utilizing runiac_environment to create multiple environments from a single source of configuration files
-   scatter/gather step strategy

-   seperate steps concurrently and then tying together in a step afterwards
-   use cautiously compared to a single step

-   use cases:

-   a single step becoming unwieldily large
-   separating provider configurations

-   Running terraform state commands
-   Is there value in testing infrastructure as code?
-   Design paradigm:

-   all configuration variables can be derived from a common set of contextual inputs

-   this enables portability of deployments patterns by avoiding coupling adding additional pipeline or external execution logic to pass in variables.

-   Multi-region deployments

-   how can i define different regions for different environments?

-   use the cli to override defaults in automated pipelines

-   how can i prevent duplicating code between primary and regional steps?

-   you can move all the terraform configurations to regional. runiac requires terraform to be present in the primary but it does not require any resources to be defined
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTYyNjYxMzk0Ml19
-->