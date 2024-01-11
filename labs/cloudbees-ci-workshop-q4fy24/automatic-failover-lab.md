# LAB 1-4: Active-Active Automatic Failover with CloudBees High Availability

In this exercise, you will complete the following tasks:

- Task 1: Create a Pipeline.
- Task 2: Access to the Kubernetes Web Dashboard
- Task 3: Run the Pipeline.
- Task 4: Delete the replica running the build.
- Task 5: Verify the build adoption.
- Task 6: Verify that a new replica is created.

## Create a Standalone Pipeline
- Navigate to `controller-2` and verify that it is running in developer mode.
- Create a new Pipeline `long-running-job` using the following Jenkinsfile:

```
pipeline {
    agent { label 'linux' }
    stages {
        stage('Long running job') {
            steps {
                sh '''
                env | sort
                for i in 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20
                do
                  echo "sleep #$i"
                  echo "Controller-2 long running job"
                  date
                  sleep 10s
                done
                '''
            }
        }
    }
}
```

## Accessing the Kubernetes Web Dashboard
- Open the file `token.txt` (stored in your VM Desktop), and copy the auth token for the Kubernetes Web Dashboard.
- Open the following URL using the Chrome browser https://dashboard:32000.
- Paste the token and select **Sign in** to access the Kubernetes Web Dashboard.
- Select the Kubernetes namespace `cloudbees-core` on the dropdown list at the top of the screen, and select the **Pods** entry on the **Workloads** section on the left navigation pane. This screen displays all the controller replicas.

## Run the Standalone Pipeline
- Navigate back to the `controller-2` dashboard.
- Pay attention to the name of the current replica (`controller-2-644bb84b8-hrqsh` in the example).
- Run the Pipeline and open the `Console Output`.

## Delete the Replica Running the Build
- Open the Kubernetes Web Dashboard on https://dashboard:32000. Depending on the time since your last access, you may need to insert the auth token again.
- Delete the replica (Pod) running the build (`controller-2-644bb84b8-hrqsh` in the example).

## Verify the Build Adoption
- Navigate back to the Pipeline **Console Output**. Reload the page. You can verify that, as the replica used was deleted, you have been assigned to another replica.
- Review the **Console Output** and verify that the Pipeline build has been adopted by the other `controller-2` replica.

## Verify that a New Replica is Created
- Wait a little bit and navigate to the operations center’s dashboard.
- Verify that the CloudBees CI modern platform has automatically created a new replica to replace the deleted one.
