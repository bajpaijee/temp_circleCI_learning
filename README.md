# temp_circleCI_learning
This is the repository to understand the pipeline flow from the basic


This YAML file defines a GitHub Actions workflow named release_test. The workflow is triggered by a push event to the main branch. Here's a step-by-step explanation of what the code does:

Trigger:

The workflow is triggered when there is a push to the main branch.
Permissions:

The workflow has write permissions to the repository contents.
Jobs:

The workflow consists of two jobs: check_version and release.
Job: check_version:

Runs on: ubuntu-latest
Steps:
Checkout the repository: Uses the actions/checkout@v4 action to clone the repository.
Fetch Tags: Runs git fetch --tags to fetch all tags from the remote repository.
Print Current Version:
Fetches the latest tag using git describe --tags \git rev-list --tags --max-count=1``.
Prints the current tag and sets it as an output variable current_tag.
Increment Version:
Reads the current tag and splits it into major, minor, and patch components.
Increments the patch version. If the patch version reaches 99, it resets to 0 and increments the minor version. If the minor version reaches 99, it resets to 0 and increments the major version.
Constructs the new tag and sets it as an output variable new_tag.
Job: release:

Name: Release pushed tag
Needs: check_version (this job depends on the completion of check_version)
Runs on: ubuntu-latest
Steps:
Checkout the repository: Uses the actions/checkout@v4 action to clone the repository.
Create New Tag:
Uses the new tag from the check_version job.
Creates a new tag in the local repository and pushes it to the remote repository.
Create Release:
Uses the new tag to create a GitHub release with the tag name as the title and generates release notes.
How it triggers the action:
When a push event occurs on the main branch, GitHub Actions will start this workflow.
The check_version job will run first, fetching the latest tag, incrementing the version, and setting the new tag.
Once check_version completes, the release job will run, creating a new tag in the repository and creating a GitHub release with that tag.
This workflow automates the process of versioning and releasing new versions of your code whenever changes are pushed to the main branch.