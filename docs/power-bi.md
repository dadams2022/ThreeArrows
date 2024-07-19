### Requirements Breakdown

#### Local dev
- Get local devs access to an easy to maintain development stack
  - Use docker compose and a pre-built Identifi SQL Reporting dev container 
  - Identifi staff will need to periodically provide updates to that SQL Reporting dev container image
  - Devs push completed reports `.pbix` files to a centralized Github Repo


#### CI/CD
- Push updated reports from Github to PowerBI as part of a CI/CD workflow
  - developer submits a PR to PowerBI GH Repo
  - workflow deploys new `.pbix` files to the test workspace using an azure service principal as a Github Repo Secret and the [PowerBI CLI](https://powerbi-cli.github.io/index.html) in a GH Action 
  - Once the PR is approved and testing is complete in Stage/QA a new workflow runs to deploy the new / updated reports to the customer PowerBI workspaces

  ```mermaid
  graph TD
    A[Developer submits PR and feature branch with new PowerBI .pbix file changes] --> B[PR Workflow Deploys New Reports to Stage PowerBI Workspace]
    B --> C[Testing is Complete]
    C --> D[PR Merged]
    D --> E[New Workflow Deploys New Reports to Production PowerBI Workspaces]

    subgraph PR Workflow
        B
    end

    subgraph Production Deployment
        E
    end

    E --> F[Production Workspace 1]
    E --> G[Production Workspace 2]
    E --> H[Production Workspace 3]


#### Multi-tenant PowerBI integration
Using Service Principal profiles, a profile will be matached one-to-one to a given customer DB and PowerBI workspace. 
![img](https://learn.microsoft.com/en-us/power-bi/guidance/media/develop-scalable-multitenancy-apps-with-powerbi-embedding/create-service-principal-profiles-for-each-customer-tenant.png)


- [Multi-tenant PowerBI embedding](https://learn.microsoft.com/en-us/power-bi/guidance/develop-scalable-multitenancy-apps-with-powerbi-embedding)
- [Service Principal Profiles](https://learn.microsoft.com/en-us/power-bi/developer/embedded/embed-multi-tenancy)
