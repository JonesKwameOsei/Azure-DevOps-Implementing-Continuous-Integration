# Azure-DevOps-Implementing-Continuous-Integration
## Enabling Continuous Integration with Azure Pipelines
## Overview 
In this project, we will learn how to `define build pipelines` in Azure DevOps using `YAML`. The pipelines will serve two purposes:

1. As part of the Pull Request validation process.
2. As part of the Continuous Integration implementation.

This project builds on the knowledge acquired from the previuos [project](https://github.com/JonesKwameOsei/Azure-Pipelines-Configure-agent-pools-and-understand-pipeline-styles), where we utilised YAML pipelines to implement and use self-hosted agents.  

### Include build validation as part of a Pull Request

In this session, we will include `build validation` to **validate** a `Pull Request`.
Validating a pull request with a build validation in Azure DevOps helps `catch errors early`, `ensures code quality`, and `streamlines the merge process`. It enforces best practices and improves collaboration among developers, ultimately increasing confidence in the stability and reliability of the deployed application. By automating this process, the overall development cycle is improved, reducing the risk of introducing breaking changes into the codebase.<p>

**Task 1: Import the YAML build definition**

In this task, we will import the YAML build definition that will be used as a Branch Policy to validate the pull requests.

Let's start by importing the build pipeline named `eshoponweb-ci-pr.yml`.

1. Go to Pipelines > Pipelines
2. Click on the "Create Pipeline" or "New Pipeline" button
3. Select "Azure Repos Git (YAML)"
4. Select the "eShopOnWeb" repository
5. Select "Existing Azure Pipelines YAML File"
6. Select the main branch and the `/.ado/eshoponweb-ci-pr.yml` file, then click on "Continue"

The build definition consists of the following tasks:

- **DotNet Restore**: With NuGet Package Restore you can install all your project's dependency without having to store them in source control.
- **DotNet Build**: Builds a project and all of its dependencies.
- **DotNet Test**: .Net test driver used to execute unit tests.
- **DotNet Publish**: Publishes the application and its dependencies to a folder for deployment to a hosting system. In this case, it's `Build.ArtifactStagingDirectory`.

Click the "Save" button to save the pipeline definition.

Your pipeline will take a name based on the project name. Let's rename it for identifying the pipeline better. Go to Pipelines > Pipelines and click on the recently created pipeline. Click on the ellipsis and "Rename/Move" option. Name it "eshoponweb-ci-pr" and click on "Save".<p>
![pr-ci](https://github.com/JonesKwameOsei/Azure-DevOps-Implementing-Continuous-Integration/assets/81886509/39cdfa3a-d4df-452c-90ee-c0ff0d7cae01)<p>

**Task 2: Branch Policies**

This task seeks to add policies to the main branch and only allow changes using Pull Requests that comply with the defined policies. You want to ensure that changes in a branch are reviewed before they are merged.

1. Go to Repos > Branches section.
2. On the `Mine` tab of the Branches pane, hover the mouse pointer over the main branch entry to reveal the ellipsis symbol on the right side.
3. Click the ellipsis and, in the pop-up menu, select "Branch Policies".<p>
![branch1](https://github.com/JonesKwameOsei/Azure-DevOps-Implementing-Continuous-Integration/assets/81886509/5b22b786-176c-4c99-a372-bd02107229e2)<p>

4. On the main tab of the repository settings, enable the option for "Require minimum number of reviewers". Add 1 reviewer and check the box "Allow requestors to approve their own changes" (as you are the only user in your project for the lab).<p>
![branchpolicy1](https://github.com/JonesKwameOsei/Azure-DevOps-Implementing-Continuous-Integration/assets/81886509/577cd158-7b7c-47e2-bd4a-3b70ae0b7fcd)<p>

5. On the main tab of the repository settings, in the `Build Validation` section, click "+" (Add a new build policy) and in the `Build pipeline` list, select `eshoponweb-ci-pr` then click `Save`.<p>
![branchpolicy2](https://github.com/JonesKwameOsei/Azure-DevOps-Implementing-Continuous-Integration/assets/81886509/99b9b4a0-2093-4cd8-9e50-2300068b8050)<p>

## Task 3: Working with Pull Requests

In this task, you will use the Azure DevOps portal to create a Pull Request, using a new branch to merge a change into the protected main branch.

1. Navigate to the Repos section in the eShopOnWeb navigation and click "Branches".
2. Create a new branch named "Feature01" based on the main branch.
3. Click "Feature01" and navigate to the `/eShopOnWeb/src/Web/Program.cs` file as part of the "Feature01" branch.
4. Click the "Edit" button in the top-right.
5. Make the following change on the first line:

   ```c#
   // Testing my PR
   ```

6. Click on "Commit > Commit" (leave default commit message).
7. A message will pop-up, proposing to create a Pull Request (as your "Feature01" branch is now ahead in changes, compared to "main"). Click on "Create a Pull Request".
8. In the "New pull request" tab, leave defaults and click on "Create".
9. The Pull Request will show some pending requirements, based on the policies applied to the target "main" branch:
   - At least 1 user should review and approve the changes.
   - Build validation, you will see that the build "eshoponweb-ci-pr" was triggered automatically.
10. After all validations are successful, on the top-right click on "Approve". Now from the "Set auto-complete" dropdown you can click on "Complete".
11. On the "Complete Pull Request" tab, click on "Complete Merge".









