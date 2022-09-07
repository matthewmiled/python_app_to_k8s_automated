# Intro

This project template builds on the deployment principles described in `python_app_to_k8s` by automating them with GitHub actions. 

# Process

1. Clone repo, create new local branch
2. Make desired changes to application
3. Push to new remote branch (`git push -u origin <local-branch-name>`). A new PR can be opened.
4. The `test.yml` workflow will then execute via GitHub actions (the trigger is a push to any branch apart from `main`). It will install python, install the dependencies and run `pytest` via a virtual Ubuntu machine.
5. If the tests pass, the PR can be approved and merged. A second worflow (`build.yml`) will trigger when it detects a merged PR. This workflow builds the docker image, pushes to AWS ECR, deploys to the AWS EKS Kube Cluster and sets up a cronjob. Ensure that the cluster is already set up by following the steps in  the `python_app_to_k8s` project. 

# Configuration

* Need to create a IAM role that GitHub actions can then assume, so it can 'connect' to your AWS account when it runs the AWS commands

The following configuration can/should be changed for your scenario:

* The cronjob details via `cronjob.yml`
* What you want your docker image to be called via line 18 of `build.yml`
* Your AWS account number and region via lines XXXXXXXXXX of `build.yml`
* Python version via line 21 of `test.yml` and line 1 of `Dockerfile`
