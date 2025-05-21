# GitHub Actions

## Concepts

### What are artifacts
When a workflow produces something other than a log entry, the product is called an `artifact`. For example, the Node.js build produces a Docker container that can be deployed. This artifact, the container, can be uploaded to storage by using the action `actions/upload-artifact` and later downloaded from storage by using the action `actions/download-artifact`.

Storing an artifact preserves it between jobs. Each job uses a fresh instance of a virtual machine (VM), so you can't reuse the artifact by saving it on the VM. If you need your artifact in a different job, you can upload the artifact to storage in one job and download it for the other job.

Artifacts are stored in storage space on GitHub. The space is free for public repositories and some amount is free for private repositories, depending on the account. GitHub stores your artifacts for 90 days.

### Types of GitHub actions
There are three types of GitHub actions: container actions, JavaScript actions, and composite actions.
- With container actions, the environment is part of the action's code. These actions can only be run in a Linux environment that GitHub hosts. Container actions support many different languages.
- JavaScript actions don't include the environment in the code. You'll have to specify the environment to execute these actions. You can run these actions in a VM in the cloud or on-premises. JavaScript actions support Linux, macOS, and Windows environments.
- Composite actions allow you to combine multiple workflow steps within one action. For example, you can use this feature to bundle together multiple run commands into an action, and then have a workflow that executes the bundled commands as a single step using that action.

A workflow must have at least one job. A job is a section of the workflow associated with a `runner`. A runner can be GitHub-hosted or self-hosted, and the job can run on a machine or in a container. You'll specify the runner with the `runs-on:` attribute. Here, you're telling the workflow to run this job on `ubuntu-latest`.

### GitHub-hosted versus self-hosted runners
We briefly mentioned runners as being associated with a job. A runner is simply a server that has the GitHub Actions runner application installed. In the previous workflow example, there was a runs-on: ubuntu-latest attribute within the jobs block, which told the workflow that the job will run using the GitHub-hosted runner that's running in the ubuntu-latest environment.

When it comes to runners, there are two options from which to choose: GitHub-hosted runners or self-hosted runners. If you use a GitHub-hosted runner, each job runs in a fresh instance of a virtual environment that's specified by the GitHub-hosted runner type you define, runs-on: {operating system-version}. With self-hosted runners, you need to apply the self-hosted label, its operating system, and the system architecture. For example, a self-hosted runner with a Linux operating system and ARM32 architecture would look like the following: runs-on: [self-hosted, linux, ARM32].

Each type of runner has its benefits, but GitHub-hosted runners offer a quicker, simpler way to run your workflows, albeit with limited options. Self-hosted runners are a highly configurable way to run workflows in your own custom local environment. You can run self-hosted runners on-premises or in the cloud. You can also use self-hosted runners to create a custom hardware configuration with more processing power or memory to run larger jobs, install software available on your local network, and choose an operating system not offered by GitHub-hosted runners.

### The components of GitHub Actions
There are several components that work together to run tasks or jobs within a GitHub Actions workflow. In short, an event triggers the workflow, which contains a job. This job then uses steps to dictate which actions will run within the workflow. To better see how these components work together, let's take a quick look at each one.

#### Workflows
A workflow is an automated process that you add to your repository. A workflow needs to have at least one job, and different events can trigger it. You can use it to build, test, package, release, or deploy your repository's project on GitHub.

#### Jobs
The job is the first major component within the workflow. A job is a section of the workflow that will be associated with a runner. A runner can be GitHub-hosted or self-hosted, and the job can run on a machine or in a container. You'll specify the runner with the runs-on: attribute. Here, you're telling the workflow to run this job on ubuntu-latest. We'll talk more about runners in the next unit.

#### Steps
A step is an individual task that can run commands in a job. In our preceding example, the step uses the action actions/checkout@v2 to check out the repository. What's interesting is the uses: ./action-a value. This is the path to the container action that you'll build in an action.yml file.

#### Actions
The actions inside your workflow are the standalone commands that are executed. These standalone commands can reference GitHub actions such as using your own custom actions, or community actions like the one we use in the preceding example, actions/checkout@v2. You can also run commands such as run: npm install -g bats to execute a command on the runner.

### Create a workflow
To create a workflow, you add actions to a `.yml` file in the `.github/workflows` directory in your GitHub repository.


## Links

- https://docs.github.com/en/actions/writing-workflows/choosing-when-your-workflow-runs/events-that-trigger-workflows
- https://docs.github.com/en/webhooks/webhook-events-and-payloads#pull_request