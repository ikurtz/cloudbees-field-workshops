# LAB 1-2: Improving Business Continuity with CloudBees CI Horizontal Scaling

In this exercise, you will complete the following tasks:

- Task 1: Create a new folder.
- Task 2: Verify that the new folder appears on the other replica.
- Task 3: Switch back to the original replica.
- Task 4: Create a new Pipeline.
- Task 5: Verify that the new Pipeline appears on the other replica.
- Task 6: Completely delete the folder and verify that the replicas are in sync.

## Create a New Folder
Verify that `controller-2` is running in developer mode.

- At `controller-2` 's root, create a new folder, `task-3-folder`.

## Verify the New Folder Appears on the Other Replica
- Switch to the other `controller-2` replica and verify that the folder exists and the replicas are synchronized.

## Switching Back to the Original Replica
- Navigate to **Controller-2 > Manage Jenkins > High Availability**
- Select the `Reset sticky session` button and reload the **High Availability** screen as many times as needed until you are randomly assigned to the original replica (**qtv8r** in the example screenshots).

## Create a New Standalone Pipeline
Inside the `task-3-folder` create a new Pipeline `data-synced-pipeline` using the following Jenkinsfile:

```
pipeline {
    agent { label 'linux' }
    stages {
        stage('data-sync') {
            steps {
                echo 'Pipeline for Lab 3.1'
                echo 'Data synchronization between controller replicas'
            }
        }
    }
}
```

## Verify the New Standalone Pipeline Appears on the Other Replica
- Switch to the other `controller-2` replica and verify that the Pipeline exists and the replicas are synchronized.

## Delete the Folder and Verify that the Replicas are in Sync
- Remove the `task-3-folder` from `controller-2`.
- Verify that the replicas are in sync. Visit both replicas to verify that the folder doesn’t appear.
