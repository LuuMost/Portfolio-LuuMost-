# Create S3 Buckets with Terraform

**Author:** Richard Lutendo Mudau
**Full guided walkthrough:** [NextWork Project](http://nextwork.ai/projects/aws-devops-terraform1)

**Skills:** Terraform · Infrastructure as Code (IaC) · AWS CLI · Amazon S3 · Cloud Automation

---

## Project Overview

In this project I demonstrated how to use Terraform to provision an S3 bucket — from installing and setting up Terraform, to troubleshooting errors (like installing the AWS CLI), to successfully applying my configuration to launch resources in AWS.

**Key concepts learnt:** Infrastructure as Code, configuration files, modular code, providers and plugins, state files, and lock files.

**Project reflection:** ~2 hours including demo time. The most challenging part was customizing my S3 configuration using the official Terraform documentation. The most rewarding part was seeing a local image uploaded into the S3 bucket through code.

As a complete beginner to Terraform, this project was my starting point — and it met my goals: I installed and set up Terraform and used it to launch real resources in my AWS environment.

---

## What is Terraform?

Terraform manages IT resources using code. You prepare a configuration file describing the **desired state** of your infrastructure — Terraform then builds your environment to match that state.

Terraform is one of the most popular **Infrastructure as Code (IaC)** tools. Instead of manually clicking through the AWS Management Console or running one-off CLI commands, IaC automates the whole process with code — repeatable, reviewable and version-controlled.

The main configuration file is `main.tf`, which describes the desired state of the infrastructure.

![Terraform overview](http://nextwork.ai/cheerful_lavender_brave_skunk/uploads/aws-devops-terraform1_9i0j1k2l)

## 1. My Configuration (`main.tf`)

Terraform configuration is structured in **blocks** instead of one single block of code. The advantage (modularity) is that I can update one part of `main.tf` without affecting other parts — great for team collaboration too.

My configuration had three blocks:
1. **AWS provider** block — tells Terraform which cloud platform to use
2. **S3 bucket** block — provisions the bucket with a unique name
3. **Public access block** — manages the bucket's permissions (blocking all public access)

![main.tf blocks](http://nextwork.ai/cheerful_lavender_brave_skunk/uploads/aws-devops-terraform1_ljvh9876)

## 2. Customizing the Bucket (Extension)

Using the official Terraform documentation — which shows example configurations, all available parameters, and the rules for each resource — I customized my bucket by **adding tags**. Tags let me identify which project launched the bucket, and I verified them in the **Tags** panel of the S3 console.

![Bucket tags](http://nextwork.ai/cheerful_lavender_brave_skunk/uploads/aws-devops-terraform1_ffe757cd3)

## 3. terraform init & terraform plan

- `terraform init` — initializes the project: sets up the backend for state files and installs the required plugins (the AWS provider)
- `terraform plan` — compares the resources in `main.tf` against the current infrastructure and previews what will change

![terraform plan output](http://nextwork.ai/cheerful_lavender_brave_skunk/uploads/aws-devops-terraform1_3g4h5i6j)

## 4. Fixing "no valid credentials found" — AWS CLI

My first `terraform plan` failed with **"no valid credentials found"** — my local terminal had no AWS credentials, which the AWS provider requires.

**How I fixed it:**
1. Installed the **AWS CLI** — AWS's command-line tool for interacting with my environment from my local computer instead of the console
2. Created **AWS access keys** as a way to "log in" to my AWS account over the CLI
3. Ran `aws configure` with the Access Key ID and secret — my terminal now had the credentials Terraform needed

![aws configure](http://nextwork.ai/cheerful_lavender_brave_skunk/uploads/aws-devops-terraform1_7j8k9l0m)

## 5. terraform apply — Launching the Bucket

`terraform apply` executed the changes previewed by `terraform plan`, creating **two resources** in my AWS account: the S3 bucket and its permission settings.

The sequence **init → plan → apply** is crucial: you must initialize Terraform before it can compare `main.tf` with your infrastructure — or even connect to AWS in the first place.

![terraform apply success](http://nextwork.ai/cheerful_lavender_brave_skunk/uploads/aws-devops-terraform1_1q2w3e4r)

## 6. Uploading an S3 Object Through Code

I added a new resource block to upload a local image (`Image.png`) directly into the S3 bucket. Because I had updated my configuration, I ran `terraform apply` again so Terraform could review and apply the changes.

**Validation:** I checked the S3 bucket and confirmed the new image was inside — then downloaded it back and verified it matched the original file.

![Object uploaded via Terraform](http://nextwork.ai/cheerful_lavender_brave_skunk/uploads/aws-devops-terraform1_9o0p1a2s)

---

## Key Takeaways

- IaC turns manual console work into repeatable, version-controlled code
- The `init → plan → apply` workflow gives a safe preview of every change before it touches real infrastructure
- Terraform needs valid AWS credentials (AWS CLI + `aws configure`) before it can talk to your account
- State files are how Terraform knows what already exists — they're why `plan` can show an accurate diff
- Adding a resource is just writing a new block and applying again
