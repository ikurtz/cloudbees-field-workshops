# LAB 1-3: Load Distribution with CloudBees CI Horizontal Scaling

In this exercise, you will complete the following tasks:

- Task 1: Create a Pipeline.
- Task 2: Configure the Pipeline to be executed every minute.
- Task 3: Verify that the builds are randomly distributed.
- Task 4: Change the Pipeline configuration to avoid executions every minute.

## Create a Standalone Pipeline
- Navigate to `controller-2` and verify that the controller runs in developer mode.
- Create a Pipeline named `hahs-lab-3-cron-job` using the following Jenkinsfile.

```
pipeline {
    agent { label 'linux' }
    triggers {
        cron '* * * * *'
    }
    stages {
        stage('cron') {
            steps {
                echo 'Controller-2 Cron Job'
            }
        }
    }
}
```

- Run the Pipeline once to enable the cron trigger.

## Verify that Builds are Being Randomly Distributed
- Wait a few minutes to allow CloudBees CI to run the Pipeline several times.

- Wait for several executions
- Open the **Console Output** for any of the builds, and using the **Next Build** and the **Previous Build** entries on the left navigation pane, browse between the different builds.
- Verify that each build has been randomly executed by one of the replicas.

## Disable the Standalone Pipeline to Avoid Executions Every Minute
- On the Pipeline job page, select **Configure**
- Uncheck the **Enabled** checkbox at the top right corner.
- Select **Save**.
