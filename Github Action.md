# Github Action -
- GitHub Actions is a CI/CD automation tool built into GitHub.
- It automatically performs tasks whenever something happens in your GitHub repository—for example, when you push code or create a pull request.

    Developer writes code → pushes code to GitHub → GitHub Actions automatically runs tests → builds application → creates Docker image → deploys application.

- GitHub Actions is commonly used to implement CI/CD.

# CI = Continuous Integration
- Whenever developers push code, automatically:
  - Download the latest code
  - Install dependencies
  - Run tests
  - Check code quality
  - Build the application
 
# CD = Continuous Delivery / Continuous Deployment
- After the application passes testing, GitHub Actions can automatically:
  - Build Docker images
  - Push images to Docker Hub
  - Deploy to cloud
  - Deploy to servers
  - Release software
 
# What is a Workflow?
- A workflow is an automated process defined in a YAML file.

## workflow structure

            Workflow
               │
               ├── Trigger
               │
               └── Jobs
                    │
                    ├── Job 1
                    │    ├── Step 1
                    │    ├── Step 2
                    │    └── Step 3
                    │
                    └── Job 2
                         ├── Step 1
                         └── Step 2



# Events / Triggers
- A trigger tells GitHub When should this workflow start?

        on:
          push:
            branches: [main]

- Runs only when code is pushed to main.

        on:
          pull_request:

- Runs when a Pull Request is created or updated.
- Very useful for testing code before merging.

# Manual execution
      
      on:
        workflow_dispatch:

- This allows you to manually start the workflow from GitHub.

# Jobs
- A job is a group of steps that performs a particular task.


      jobs:
        test:

- Here the job is called: test
- You could have multiple jobs:

      jobs:
      
        test:
          ...
      
        build:
          ...
      
        deploy:
          ...
      
      Conceptually:


# Runner
- Every job needs a machine on which it will execute.

      runs-on: ubuntu-latest

- This means:Run this job on a GitHub-hosted Ubuntu machine.

- Common runners include:

ubuntu-latest
windows-latest
macos-latest

- For most Python/AI CI/CD projects, you'll commonly see: runs-on: ubuntu-latest

# Steps
A job contains steps.

      steps:
      
        - name: Get Code
          uses: actions/checkout@v4
      
        - name: Install Python
          uses: actions/setup-python@v5
      
        - name: Install Dependencies
          run: pip install -r requirements.txt
      
        - name: Run Tests
          run: pytest

- Each step performs one operation.

# uses vs run
- This is extremely important. There are two common ways to perform an action.
- **uses**
- uses means: Use an existing GitHub Action.

      uses: actions/checkout@v4

- You're using an already-created action.

- **run**
- run means:Execute a command on the runner
  
        run: pip install -r requirements.txt


