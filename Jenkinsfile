pipeline {
    environment{
        registry = "elienetomedia/devops-exercise"
        regestryCredential = 'dockerhub-credentials	'
        PROJECT_ID = 'eli-poc'
        CLUSTER_NAME = 'devops-exercise-cluster'
        LOCATION = 'us-central1-c'
        CREDENTIALS_ID = 'gke'
    }
    agent any
        stages{
            stage("run tests") {
                agent{ 
                    docker { 
                        image 'python:3.8'
                        args '-u root:sudo'
                    } 
                }
                steps {
                    sh 'pip install --user -r requirements.txt'
                    sh 'python test.py'
                }
                post {
                    always {
                        junit 'results.xml'
                    }
                }
            }
            stage('build image') {
                steps {
                    script {
                        dockerImage = docker.build registry + ":$BUILD_NUMBER"
                    }
                }
            }
            stage('deploy image') {
                steps {
                    script {
                        docker.withRegistry('', regestryCredential) {
                            dockerImage.push()
                        }
                    }
                }
            }
            stage('Deploy to GKE') {
                steps{
                    // sh "sed -i 's/hello:latest/hello:${env.BUILD_ID}/g' deployment.yaml"
                    step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'k8s', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
                }
            }
        }
}