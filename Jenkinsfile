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
                    sh "sed -i 's/Hello world latest version/Hello world latest version:${env:BUILD_NUMBER}/g' index.html"
                    script {
                        dockerImageBuildNumber = docker.build registry + ":$BUILD_NUMBER"
                        dockerImageLatest = docker.build registry + ":latest"
                    }
                }
            }
            stage('deploy image to Docker Hub') {
                steps {
                    script {
                        docker.withRegistry('', regestryCredential) {
                            dockerImageBuildNumber.push()
                            dockerImageLatest.push()
                        }
                    }
                }
                post {
                    always {
                        archiveArtifacts artifacts: 'index.html', followSymlinks: false
                    }
                }
            }
            stage('Deploy to GKE') {
                steps{
                    sh "sed -i 's@/devops-exercise:latest/devops-exercise:${env.BUILD_NUMBER}/@g' k8s/devops-exercise-deployment.yaml"
                    step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'k8s', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
                }
                post {
                    always {
                        archiveArtifacts artifacts: 'k8s/devops-exercise-deployment.yaml', followSymlinks: false
                    }
                }
            }
        }
}