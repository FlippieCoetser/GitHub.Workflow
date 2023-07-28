# Code Repository and Branching Strategy

This document provides a guideline for the code repository and branching strategy you should follow.
To interact with GitHub and follow along you will need to install [git](https://git-scm.com/) on your local machine and ensure you have an account registered on [GitHub](www.github.com).

Depending on your role within the team you will use one of the following workflows:

1. Product Management workflow

Used by Product Owners and Business Analysts to sign off user stories.

2. Development workflow

Used by Developers to implement user stories.

Before diving into the details or the workflows, let first look at the basic git commands applicable to both workflows.

> Note: We will use an example repository throughout this document. This will allow you to follow along and practice the commands

## Basic Git Commands

### Cloning a Repository

The first step when working with a GitHub code repository is to create a clone. Cloning a repository essentially means you create a copy of what is in GitHub on your local machine.

To create a local clone of a GitHub repository follow these steps:

1. Using your browser navigate to the repository: `https://github.com/{Username}/{RepositoryName}`.
   Notice the URL pattern: you will need the `username` of the owner of the repository and the `repository name`.

![Enterprise Application Hierarchy](/Design%20Assets/GitHub.Repository.png)

2. Verify that the correct repository is loaded and use the `< > Code` button to copy the repository url

![Enterprise Application Hierarchy](/Design%20Assets/GitHub.Code.png)

3. Navigate on your local machine to a `directory` where you would like to store the repository: `cd {drive}:\{directory}\`.

```bash
cd C:\Data\
```

4. Clone the repository using the `git clone` command follow by pasting the `url` you copied in step 3.

```bash
git clone https://github.com/FlippieCoetser/r.template.git
```

5. After successfully cloning the repository a new directory will be created.
   Change directory into the newly created directory and list the contents of the directory.

```bash
cd r.template
dir
```

6. Verify the content in this folder match the content in GitHub.

![Enterprise Application Hierarchy](/Design%20Assets/GitHub.Clone.png)

if the content do not match, follow the update local repository section below.

### Update local repository

The content of the local repository will not update automatically when changes are made to the cloud repository.
It is good practice before you start working to update your local repository with the latest changes from the cloud repository.
This is achieved with the `git pull` command.

1. Ensure you are in the correct directory and run the `git pull` command.

```bash
git pull
```

## Development Workflow

### Create a new branch

When agile requirements are used, it is common for the issue in GitHub to reference a user story.
Essentially the user story defines a new feature or a bug fix which the Product Owner wanted implemented.

The development team typically discuss the user story with the Product Owner to align expectations and scope.
The team may also break down the user story into smaller tasks which are then assigned to different developers.

> Example:  
> As a `user`,  
> When I want to plan my day,  
> I want to `see` a list of `todos`,  
> So that I can prioritize my work.

The example above may seem simple, but in reality touch many parts of the application. For example an User Interface needs to be designed or a database needs to be created.

Typically the smaller tasks will fall under one of the below categories:

INFRA, PROVISION, RELEASE, DATA, BROKERS, FOUNDATIONS, VIEWS, PROCESSING, ORCHESTRATION, COORDINATIONS, MANAGEMENT, AGGREGATIONS, CONTROLLERS, BASE, COMPONENTS, PAGES, ACCEPTANCE, INTEGRATIONS, CODE RUB, DOCUMENTATION, CONFIG, MINOR FIX, MEDIUM FIX, MAJOR FIX, REVIEW, STANDARD, DESIGN, BUSINESS, UNKNOWN

The next step is for the developer to create a new branch for the assigned task.
For example, assume a developer is assigned to design a new database table to support the storage of todos.

The categorization of the task is `DATA` and the business entity is `todo` and the action is `retrieve`.

A new local branch can be created using this above information as follows: `users/{GitHubUsername}/{Type}-{Entity}-{Action}`.

based on the above example the branch name will be: `users/FlippieCoetser/Data-Todo-Retrieve`.

1. Create a new branch

```bash
git checkout -b users/FlippieCoetser/Data-Todo-Retrieve
```

> Note: It is not always possible to follow this exact naming pattern. However, it is important to ensure the branch starts with `users/{GitHubUsername}/`. The last part of the branch name should describe the work to be done. If you are unsure discuss with the team or consult the product owner.

2. Push the new branch to the cloud repository

The command you executed in step 1 will switch to the newly created branch automatically.
Meaning any changes made to the code will be made to the new branch.
However, the new branch only exists on your local machine.
To make the new branch also available in the cloud repository, you need to push the new branch to the cloud repository.

```bash
git push --set-upstream origin users/FlippieCoetser/Data-Todo-Retrieve
```

3. Verify the new branch is available in the cloud repository

![Enterprise Application Hierarchy](/Design%20Assets/GitHub.Branches.png)

### Commit Changes

A test driven development (TDD) approach is used to ensure the code is of high quality.
This means that code change happens in pairs:

1. A failing unit test is written
2. The code is written to make the unit test pass

It is very important that you program with intent and purpose. Commit message should reflect this. If you want to explore or experiment with an idea, create a new branch for this purpose. Once you are happy with the results and comfortable to implement your idea switch back to the branch where the implementation is needed and follow the TDD approach.

The rationale for not exploring or experimenting in the branch where the implementation is needed is to ensure the Product Owner can track the progress of the user story and to ensure the main branch is not polluted with unnecessary code.

The commit message should be clear and concise. It should describe the work done and the status of the work.
It is recommended to use the following commit message structure: `{Type}: {TestCondition} -> {StatusStatus}`

Again this might not always be possible. For example if you want to add documentation or configuration files, you will not write unit tests. In such case provide a clear commit message which describe what you have done.

Below is an example of work done to implement the `Retrieve` operation in the `Todo` broker in an R code base.

1. Create a new unit test which test if a `Todo` broker exists.

> note: the code snippet below an example and based on R. The same principle applies to other languages.

```r
# test-Broker.Todo.R
describe('Given Broker.Todo', {
  it('exists', {
    #Given
    Broker.Todo |> is.null() |> expect_equal(FALSE)
  })
})
```

2. Ensure the test fails and Stage changes

```bash
git add tests/testthat/test-Broker.Todo.R
```

3. Commit changes with a clear and concise message

```bash
git commit -m "BROKER: Todo.Broker Exist -> Fail"
```

4. Push the changes to the cloud repository

```bash
git push
```

5. Options step: Check if the changes are available in the cloud repository.
   Navigate to the cloud repository and change the branch to the newly create one and click commits link.
   You should see the commit message and the files changed.

![Enterprise Application Hierarchy](/Design%20Assets/GitHub.Commit.png)

6. Write the code to make the test pass

```r
# Broker.Todo.R
Broker.Todo <- \() {}
```

7. Ensure the test pass and Stage changes

```bash
git add Broker.Todo.R
```

8. Commit changes with a clear and concise message

```bash
git commit -m "BROKER: Todo.Broker Exist -> Pass"
```

9. Push the changes to the cloud repository

```bash
git push
```

10. You should not see a failing test is followed by a passing test directly in the comment message.

![Enterprise Application Hierarchy](/Design%20Assets/GitHub.Pass.png)

The above steps are repeated until the user story is completed.

### Creating a Pull Request

Once the user story is completed, the Product owner will perform user acceptance testing and hopefully approve the new feature.

Assuming the Product Owner has signed off on the user story, the branch is ready to be merged back into the main branch.
To do so a pull request is created in GitHub. Although the command-line interface can be used to create a pull request, the Product Owner will typically create the pull request via the GitHub web interface.

1. Using the browser navigate to the repository and select `Pull requests` in the tab strip.
   You should already see a suggestion from GitHub to create a pull request.

![Enterprise Application Hierarchy](/Design%20Assets/GitHub.PullRequest.png)

**Rather click New pull request** than accepting the suggestion. This will ensure you consciously select the correct branch to merge into the main branch.

2. Select the correct branch to merge into the main branch and click `Create pull request`.

![Enterprise Application Hierarchy](/Design%20Assets/GitHub.PullRequest2.png)

3. Update the pull request `Description` with a clear and concise message and click `Create pull request`.

![Enterprise Application Hierarchy](/Design%20Assets/GitHub.PullRequest3.png)

4. GitHub will automatically run all unit tests and check if the code can be merged into the main branch without conflicts.
   To merge the changes into the main branch, click `Merge pull request`.

![Enterprise Application Hierarchy](/Design%20Assets/GitHub.Merge.png)

### Delete Branch

Once the code has been merged into the main branch, the branch can be deleted.
This is a two step process:

1. Delete branch from cloud repository

```bash
git remote update origin --prune
```

use `git branch --all` to list all branches: local and remote.

2. Delete the branch from the local repository

```bash
git branch -d users/FlippieCoetser/Data-Todo-Retrieve
```

## Product Management Workflow

Product Owners involved in singing off user stories will typically only use a `Git Clone`,`Git switch`, `Git Pull` commands in addition to the actions performed in the GitHub web interface.

We already covered the `Git Clone` and `Git Pull` commands in the previous section. So let's look at `Git switch`.
After cloning the code repository, the Product Owner may want to first see the new feature before signing off on it.

To do this, the Product Owner will follow the below steps:

1. Clone the repository if not already done

```bash
git clone https://github.com/FlippieCoetser/r.template.git
```

1. Switch to the branch containing the new feature

```bash
git switch users/FlippieCoetser/Data-Todo-Retrieve
```

2. Pull all changes from the cloud repository

```bash
git pull
```

3. Run the application and test the new feature

After running the application from this branch, the Product Owner can then sign off on the user story and approve the pull request. The pull request will then be merged into the main branch and the branch can be deleted.

4. Create a pull request via the GitHub web interface
5. Merge the pull request via the GitHub web interface
6. Delete the branch in the GitHub web interface and locally
