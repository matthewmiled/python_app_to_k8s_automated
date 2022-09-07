# Intro

This project template builds on the deployment principles described in `python_app_to_k8s` by automating them with GitHub actions. 

# Workflow

1. Clone repo, create new local branch
2. Make desired changes to application
3. Push to new remote branch (`git push -u origin <local-branch-name>`). A new PR can be opened.
4. The `test.yml` workflow will then execute via GitHub actions (the trigger is a push to any branch apart from `main`). It will install python, install the dependencies and run `pytest` via a virtual Ubuntu machine.
5. If the tests pass, the PR can be approved and merged. A second workflow (`build.yml`) will trigger when it detects a merged PR. This workflow builds the docker image, pushes to AWS ECR, deploys to the AWS EKS Kube Cluster and sets up a cronjob. Ensure that the cluster is already set up by following the steps in  the `python_app_to_k8s` project. 

# User Configuration

1. Firsly, create a IAM role and OpenID connect identify in AWS. Benoit Boure has a great tutorial on this:

https://benoitboure.com/securely-access-your-aws-resources-from-github-actions

2. Create an ECR repo for your project/application in the AWS console

3. Change the ENV VARS in `build.yaml` for your requirements (one of the ENV VARS is the ECR repo name)

4. Also need to copy the IAM role ARN and manually paste into the section below in `build.yaml` (the ENV VARS don't work for this step for some reason):

```
- name: Configure AWS credentials
  uses: aws-actions/configure-aws-credentials@v1
  with:
    role-to-assume: <YOUR_IAM_ARN> # e.g. arn:aws:iam::12345678900:role/github-actions-role
    aws-region: <YOUR_AWS_REGION>
```
5. Create a Kubernetes cluster using EKS by following the steps in `python_app_to_k8s`. A config file for this cluster should then be created on your local machine at `$HOME/.kube/config`.

6. Convert this config file to BASE64 by using 'cat $HOME/.kube/config | base64' 

