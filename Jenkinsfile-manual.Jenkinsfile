pipeline {
    environment{
        PROJECT_ID = 'eli-poc'
        CLUSTER_NAME = 'devops-exercise-cluster'
        LOCATION = 'us-central1-c'
        CREDENTIALS_ID = 'gke'
    }
    agent any
    stages{
        stage('Deploy to GKE') {
            steps{
                sh "sed -i 's/devops-exercise:latest/devops-exercise:${env.TAG_NAME}/g' k8s/devops-exercise-deployment.yaml"
                step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'k8s', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
            }
        }
    }
    post {
        success {
            echo 'Pipeline completed successfully'
        }
        failure {
            echo 'Pipeline finished with an error, see the logs for more information'
        }
    }
    
}