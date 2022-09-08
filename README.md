# Intro

This project template builds on the deployment principles described in `python_app_to_k8s` by automating them with GitHub actions. It is assumed that you have followed the steps in that repo to create an EKS cluster. Make sure that your cluster and ECR repos are set to the same region.

# Workflow

1. Clone repo, create new local branch
2. Make desired changes to application
3. Push to new remote branch (`git push -u origin <local-branch-name>`). A new PR can be opened.
4. The `test.yml` workflow will then execute via GitHub actions (the trigger is a push to any branch apart from `main`). It will install python, install the dependencies and run `pytest` via a virtual Ubuntu machine.
5. If the tests pass, the PR can be approved and merged. A second workflow (`build.yml`) will trigger when it detects a merged PR. This workflow builds the docker image, pushes to AWS ECR, deploys to the AWS EKS Kube Cluster and sets up a cronjob. Ensure that the cluster is already set up by following the steps in  the `python_app_to_k8s` project. 

# User Configuration

1. Create an ECR repo for your project/application in the AWS console

2. Save your `AWS_ACCESS_KEY`, `AWS_SECRET_ACCESS_KEY` and `AWS_REGION` as secret repo environments in the GitHub repo settings menu

3. Change the following for your needs in `build.yaml`:

* `ECR_REPOSITORY` - the name of your repo that you created in step 1. Each app/project should have it's own repo where images are saved
* `EKS_CLUSTER` - the name of your cluster

4. Configure `deployment.yaml` for your needs. You will need to change the `image` so it points to your account number and ECR_REPOSITORY. 

5. The `deployment.yaml` is currently set to deploy a cronjob that runs every minute, but it is suspended. If you change this at all (or change the name of the file or cronjob), you may need to update the deployment commands at the bottom of the `build.yaml`. 
 

