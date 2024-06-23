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

**Task 3: Working with Pull Requests**

In this task, we will utilise the Azure DevOps portal to create a Pull Request, using a new branch to merge a change into the protected main branch.

1. Navigate to the Repos section in the eShopOnWeb navigation and click "Branches".
2. Create a new branch named "Feature01" based on the main branch.<p>
![newbranch](https://github.com/JonesKwameOsei/Azure-DevOps-Implementing-Continuous-Integration/assets/81886509/ccbb7ced-fb13-444b-8abb-68af18d25539)<p>

3. Click "Feature01" and navigate to the `/eShopOnWeb/src/Web/Program.cs` file as part of the "Feature01" branch.
4. Click the "Edit" button in the top-right.
5. Make the following change on the first line:

   ```c#
   // Testing my PR
   ```
![newbranch](https://github.com/JonesKwameOsei/Azure-DevOps-Implementing-Continuous-Integration/assets/81886509/a138eb9b-6026-4eb0-9f5d-4e639376f176)<p>

6. Click on "Commit > Commit" (leave default commit message).<p>
![newbranch-committed](https://github.com/JonesKwameOsei/Azure-DevOps-Implementing-Continuous-Integration/assets/81886509/2b55f948-1bf5-4aea-996f-dcc429f34f82)<p>

7. A message will pop-up, proposing to create a Pull Request (as your "Feature01" branch is now ahead in changes, compared to "main"). Click on "Create a Pull Request".
8. In the "New pull request" tab, leave defaults and click on "Create".<p>
![newbranch-committed2](https://github.com/JonesKwameOsei/Azure-DevOps-Implementing-Continuous-Integration/assets/81886509/2d3244b3-41b2-4ea2-bf7f-3a8de663458d)

9. The Pull Request will show some pending requirements, based on the policies applied to the target "main" branch:
   - At least 1 user should review and approve the changes.
   - Build validation, you will see that the build "eshoponweb-ci-pr" was triggered automatically.
10. After all validations are successful, on the top-right click on "Approve". Now from the "Set auto-complete" dropdown you can click on "Complete".
11. On the "Complete Pull Request" tab, click on "Complete Merge".<p>
![Pull-request-completed](https://github.com/JonesKwameOsei/Azure-DevOps-Implementing-Continuous-Integration/assets/81886509/7cfedf84-4060-4f88-a75b-d865bd47da48)<p>

### Configure CI Pipeline as Code with YAML

In this exercise, you will configure CI Pipeline as code with YAML.

**Task 1: Import the YAML build definition for CI**

In this task, we aim at adding the YAML build definition that will be used to implement the Continuous Integration.

Let's start by importing the CI pipeline named [eshoponweb-ci.yml](https://github.com/MicrosoftLearning/eShopOnWeb/blob/main/.ado/eshoponweb-ci.yml).

1. Go to Pipelines > Pipelines.
2. Click on the "New Pipeline" button.
3. Select "Azure Repos Git (YAML)".
4. Select the "eShopOnWeb" repository.
5. Select "Existing Azure Pipelines YAML File".
6. Select the main branch and the `/.ado/eshoponweb-ci.yml` file, then click on "Continue".<p>
The pipeline workflows:<p>
```
#NAME THE PIPELINE SAME AS FILE (WITHOUT ".yml")
# trigger:
# - main

resources:
  repositories:
    - repository: self
      trigger: none

stages:
- stage: Build
  displayName: Build .Net Core Solution
  jobs:
  - job: Build
    pool:
      vmImage: ubuntu-latest
    steps:
    - task: DotNetCoreCLI@2
      displayName: Restore
      inputs:
        command: 'restore'
        projects: '**/*.sln'
        feedsToUse: 'select'

    - task: DotNetCoreCLI@2
      displayName: Build
      inputs:
        command: 'build'
        projects: '**/*.sln'
    
    - task: DotNetCoreCLI@2
      displayName: Test
      inputs:
        command: 'test'
        projects: 'tests/UnitTests/*.csproj'
    
    - task: DotNetCoreCLI@2
      displayName: Publish
      inputs:
        command: 'publish'
        publishWebProjects: true
        arguments: '-o $(Build.ArtifactStagingDirectory)'
    
    - task: PublishPipelineArtifact@1
      displayName: Publish Artifacts ADO - Website
      inputs:
        targetPath: '$(Build.ArtifactStagingDirectory)'
        artifact: 'Website'
        publishLocation: 'pipeline'

    - task: PublishPipelineArtifact@1
      displayName: Publish Artifacts ADO - Bicep
      inputs:
        targetPath: '$(Build.SourcesDirectory)/infra/webapp.bicep'
        artifact: 'Bicep'
        publishLocation: 'pipeline'
```
The CI definition consists of the following tasks:

- **DotNet Restore**: With NuGet Package Restore you can install all your project's dependency without having to store them in source control.
- **DotNet Build**: Builds a project and all of its dependencies.
- **DotNet Test**: .Net test driver used to execute unit tests.
- **DotNet Publish**: Publishes the application and its dependencies to a folder for deployment to a hosting system. In this case, it's `Build.ArtifactStagingDirectory`.
- **Publish Artifact - Website**: Publish the app artifact (created in the previous step) and make it available as a pipeline artifact.
- **Publish Artifact - Bicep**: Publish the infrastructure artifact (Bicep file) and make it available as a pipeline artifact.<p>

**Task 2: Enable Continuous Integration**

The default build pipeline definition doesn't enable Continuous Integration.

1. Click the "Edit" button in the top-right.
2. Now, you need to replace the `# trigger:` and `# - main` lines with the following code:

   ```yaml
   trigger:
     branches:
       include:
       - main
     paths:
       include:
       - src/web/*
   ```
Updated workflows should look like this:
```
name: eshoponweb-ci
trigger:
   branches:
     include:
     - main
   paths:
     include:
     - src/web/*

resources:
  repositories:
    - repository: self
      trigger: none

stages:
- stage: Build
  displayName: Build .Net Core Solution
  jobs:
  - job: Build
    pool:
      vmImage: ubuntu-latest
    steps:
    - task: DotNetCoreCLI@2
      displayName: Restore
      inputs:
        command: 'restore'
        projects: '**/*.sln'
        feedsToUse: 'select'

    - task: DotNetCoreCLI@2
      displayName: Build
      inputs:
        command: 'build'
        projects: '**/*.sln'
    
    - task: DotNetCoreCLI@2
      displayName: Test
      inputs:
        command: 'test'
        projects: 'tests/UnitTests/*.csproj'
    
    - task: DotNetCoreCLI@2
      displayName: Publish
      inputs:
        command: 'publish'
        publishWebProjects: true
        arguments: '-o $(Build.ArtifactStagingDirectory)'
    
    - task: PublishPipelineArtifact@1
      displayName: Publish Artifacts ADO - Website
      inputs:
        targetPath: '$(Build.ArtifactStagingDirectory)'
        artifact: 'Website'
        publishLocation: 'pipeline'

    - task: PublishPipelineArtifact@1
      displayName: Publish Artifacts ADO - Bicep
      inputs:
        targetPath: '$(Build.SourcesDirectory)/infra/webapp.bicep'
        artifact: 'Bicep'
        publishLocation: 'pipeline'
```
This will automatically trigger the build pipeline if any change is made to the main branch and the web application code (the `src/web` folder).

3. Since you enabled Branch Policies, you need to pass by a Pull Request in order to update the code.
4. Click the "Save" button (not "Save and run") to save the pipeline definition.
5. Select "Create a new branch for this commit".
6. Keep the default branch name and "Start a pull request" checked.
7. Click on "Save".
8. Your pipeline will take a name based on the project name. Let's rename it for identifying the pipeline better. Go to Pipelines > Pipelines and click on the recently created pipeline. Click on the ellipsis and "Rename/Move" option. Name it "eshoponweb-
9. ci" and click on "Save".<p>
![rename-ci](https://github.com/JonesKwameOsei/Azure-DevOps-Implementing-Continuous-Integration/assets/81886509/ba0da64e-bf1d-4972-9942-4f773a75f1d8)<p>

10. Go to Repos > Pull Requests.
11. Click on the "Update eshoponweb-ci.yml for Azure Pipelines" pull request.
12. After all validations are successful, on the top-right click on "Approve". Now you can click on "Complete".
13. On the "Complete Pull Request" tab, Click on "Complete Merge".<p>
![ci-updated](https://github.com/JonesKwameOsei/Azure-DevOps-Implementing-Continuous-Integration/assets/81886509/87e4392d-e865-4287-b05f-fc76a53f34b9)<p>

**Task 3: Test the CI pipeline**

In this task, our goal is to create a `Pull Request`, using a `new branch` to merge a change into the protected **main** branch and automatically trigger the CI pipeline.

1. Navigate to the Repos section.
2. Create a new branch named "Feature02" based on the main branch.
3. Click the new "Feature02" branch.
4. Navigate to the `/eShopOnWeb/src/Web/Program.cs` file and click "Edit" in the top-right.
5. Remove the first line:

   ```c#
   // Testing my PR
   ```

6. Click on "Commit > Commit" (leave default commit message).
7. A message will pop-up, proposing to create a Pull Request (as your "Feature02" branch is now ahead in changes, compared to "main").<p>
![new-feat-branch](https://github.com/JonesKwameOsei/Azure-DevOps-Implementing-Continuous-Integration/assets/81886509/bac6b466-c86d-4d28-aec2-8ee2d9b74770)<p>

8. Click on "Create a Pull Request".<p>
![new-feat-branch-commit](https://github.com/JonesKwameOsei/Azure-DevOps-Implementing-Continuous-Integration/assets/81886509/c70d6625-fdd1-45a3-b248-79c27dc0e6f3)<p>

9. In the "New pull request" tab, leave defaults and click on "Create".
10. The Pull Request will show some pending requirements, based on the policies applied to the target "main" branch.<p>
![new-feat-branch-merge](https://github.com/JonesKwameOsei/Azure-DevOps-Implementing-Continuous-Integration/assets/81886509/bf7336db-be49-488f-94b4-d72e8a187ee1)<p>

11. After all validations are successful, on the top-right click on "Approve". Now from the "Set auto-complete" dropdown you can click on "Complete".
12. On the "Complete Pull Request" tab, Click on "Complete Merge".<p>
![new-feat-branch-merged](https://github.com/JonesKwameOsei/Azure-DevOps-Implementing-Continuous-Integration/assets/81886509/bed984f8-63aa-4e25-8487-04b92ae5d091)<p>

13. Go back to Pipelines > Pipelines, you will notice that the build "eshoponweb-ci" was triggered automatically after the code was merged.
14. Click on the "eshoponweb-ci" build then select the last run.<p>
![ci-run](https://github.com/JonesKwameOsei/Azure-DevOps-Implementing-Continuous-Integration/assets/81886509/0a3c50e5-3a56-4158-b3df-27085727afce)<p>

16. After its successful execution, click on "Related > Published" to check the published artifacts:
    - Bicep: the infrastructure artifact.
    - Website: the app artifact.<p>

![ci-run-artifact](https://github.com/JonesKwameOsei/Azure-DevOps-Implementing-Continuous-Integration/assets/81886509/0bfcc6d5-4490-4f33-be35-bf51dd636fed)<p>

### Conclusion
During this lab, I set up pull request validation by creating a build definition and configured the `CI pipeline` using YAML as code in Azure DevOps.





