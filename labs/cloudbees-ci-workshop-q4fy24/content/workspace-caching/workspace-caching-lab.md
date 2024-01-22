# LAB 2: Increasing Velocity with Workspace Caching for CloudBees CI

In this exercise, you will complete the following tasks:
- Verify that the CloudBees Cache Step Plugin, Artifact Manager on S3 Plugin, and the AWS Global Configuration Plugin have been installed on your Managed Controller.
- Verify that the Workspace Caching configuration has been successfully setup and validated.
    - Verify Artifact Manager on S3 Plugin has been successfully setup
    - Verify the target S3 bucket and its configuration in both CloudBees CI and AWS
- Run a sample Maven Pipeline that does not yet take advantage of Workspace Caching and review the build logs in the console output
- Run a sample Maven Pipeline that does take advantage of Workspace Caching and review the build logs in the console output
- Identify the differences in total build duration between the two aforementioned builds to learn how Workspace Caching can make a significant difference in improving your business

## Introducing Workspace Caching for CloudBees CI
The [CloudBees Cache Step Plugin](https://docs.cloudbees.com/plugins/ci/cloudbees-cache-step) provides `writeCache` and `readCache` Pipeline steps to use separate storage as caches for workspaces. This is useful for builds running on cloud agents that start with empty caches of build tools, or when builds involve temporary files that take much longer to generate than they take to download.

The [CloudBees S3 Cache Plugin](https://docs.cloudbees.com/plugins/ci/cloudbees-s3-cache) provides a cache implementation for these steps based on [AWS S3](https://aws.amazon.com/s3/).

> [!IMPORTANT]
> Using AWS S3 to store workspace caches will incur additional costs from AWS. Refer to the [AWS documentation](https://aws.amazon.com/s3/pricing/?nc=sn&loc=4) for S3 pricing information.

## Verify Configuration of Workspace Caching
Workspace caching is disabled by default. As an administrator, you can enable this setting on the System configuration page. On `Controller-1`, confirm that workspace caching has been properly configured by first selecting **Manage Jenkins > System**. Navigate to **Workspace Caching** and review the current configuration for the **Cache Implementation**. The **S3 Cache** from the **Cache Manager** list should be selected, as shown in the illustration below.

![Selecting the S3 Cache](https://docs.cloudbees.com/docs/cloudbees-ci/latest/pipelines/_images/workspace-cache-screenshots/workspace-caching-global.35d6b6c.png)

The S3 cache uses the configuration of the [Artifact Manager on S3 plugin](https://docs.cloudbees.com/plugins/ci/artifact-manager-s3). This plugin permits you to archive artifacts in an S3 Bucket, where there is less need to be concerned about the disk space used by artifacts. We will review the AWS configuration for the S3 bucket we'll be targeting as the cache just after we validate that workspace caching has been enabled for `Controller-1`.

> [!TIP]
> Under **Advanced Options** you can also configure which jobs are able to use the cache with the **Job Include Pattern** and **Job Exclude Pattern** fields.

The **Job Include Pattern** allows you to configure specific jobs that are able to use the configured cache using a comma-separated Ant-style pattern. For example:

- `**` matches all jobs.
- `my-project/**` matches all jobs in the folder `my-project`.
- `*e*` matches all jobs whose name contains the letter **e** that are not in another folder.

This field is empty, which means that all jobs are included by default.

The **Job Exclude Pattern** allows you to exclude an included job. The syntax is the same as the **Job Include Pattern**. However, an empty value means that nothing is excluded that would otherwise be included (for example, if both fields are empty, the workspace caching feature is enabled for all jobs in the CloudBees CI instance).

## Configuring the Artifact Manager for S3
For the CloudBees S3 Cache plugin specifically, the permissions `s3:PutObject`, `s3:GetObject`, `s3:DeleteObject`, and `s3:ListBucket` are needed. For more information on configuring S3, refer to the [Artifact Manager on S3 plugin](https://docs.cloudbees.com/plugins/ci/artifact-manager-s3) documentation.

Let's take a look at what we need to configure in CloudBees CI to connect to our target AWS S3 bucket for use in workspace caching:

1. Go to **Manage Jenkins > Configure System**.
2. In the **Artifact Management for Builds section**, validate that **Amazon S3** is selected as the Cloud Provider for artifact storage:


3. Navigate to **Manage Jenkins > Amazon Web Services (AWS) Configuration** to validate the name of the target S3 bucket, `cb-se-workspace-caching-demo` and the corresponding AWS IAM credentials needed to connect to it. Let's start by reviewing the bucket configuration:


4. After reviewing the S3 bucket settings, let's validate that we have configured the proper credentials before testing the connection to the `cb-se-workspace-caching-demo` S3 bucket:

>[!NOTE]
> Your AWS account must have the right IAM permissions to access the S3 Bucket, and must be able to `list`, `get`, `put`, and `delete` objects in the S3 Bucket.



5. It's time to validate the connection from `Controller-1` in CloudBees CI to the S3 bucket we have configured. Directly under the S3 bucket settings, click the **Validate S3 Bucket configuration**. As a result, the following message should be returned, indicating we're ready to use our new S3 bucket to cache build artifacts:


6. Upon successful validation, we've completed the configuration for our Artifact Manager on S3.







