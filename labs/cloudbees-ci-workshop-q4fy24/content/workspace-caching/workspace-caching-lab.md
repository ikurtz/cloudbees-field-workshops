# LAB 2: Increasing Velocity with Workspace Caching for CloudBees CI

In this exercise, you will complete the following tasks:
- Verify that the CloudBees Cache Step Plugin, Artifact Manager on S3 Plugin, and the AWS Global Configuration Plugin have been installed on your Managed Controller.
- Verify that the Workspace Caching configuration has been successfully setup and validated.
- Run a sample Maven Pipeline that does not take advantage of Workspace Caching and review the build logs in the console output
- Run a sample Maven Pipeline that does take advantage of Workspace Caching and review the build logs in the console output
- Identify the differences in total build duration between the two aforementioned builds to learn how Workspace Caching can make a significant difference in improving your business

## Introducing Workspace Caching for CloudBees CI
The [CloudBees Cache Step Plugin](https://docs.cloudbees.com/plugins/ci/cloudbees-cache-step) provides `writeCache` and `readCache` Pipeline steps to use separate storage as caches for workspaces. This is useful for builds running on cloud agents that start with empty caches of build tools, or when builds involve temporary files that take much longer to generate than they take to download.

The [CloudBees S3 Cache Plugin](https://docs.cloudbees.com/plugins/ci/cloudbees-s3-cache) provides a cache implementation for these steps based on [AWS S3](https://aws.amazon.com/s3/).

> [!IMPORTANT]
> Using AWS S3 to store workspace caches will incur additional costs from AWS. Refer to the [AWS documentation](https://aws.amazon.com/s3/pricing/?nc=sn&loc=4) for S3 pricing information.

## Verify Configuration of Workspace Caching
Workspace caching is disabled by default. As an administrator, you can enable this setting on the System configuration page. On `Controller-1`, confirm that workspace caching has been properly configured by first selecting **Manage Jenkins > System**. Navigate to **Workspace Caching** and review the current configuration for the **Cache Implementation**. The **S3 Cache** from the **Cache Manager** list should be selected.

![Selecting the S3 Cache](https://docs.cloudbees.com/docs/cloudbees-ci/latest/pipelines/_images/workspace-cache-screenshots/workspace-caching-global.35d6b6c.png)

The S3 cache uses the configuration of [Artifact Manager on S3 plugin](https://docs.cloudbees.com/plugins/ci/artifact-manager-s3). Refer to [Cloud ready Artifact Manager for AWS](https://docs.cloudbees.com/docs/cloudbees-ci/latest/cloud-reference-architecture/ra-for-aws/#ams3) for more information.

> [!TIP]
> Under **Advanced Options** you can also configure which jobs are able to use the cache with the **Job Include Pattern** and **Job Exclude Pattern** fields.







