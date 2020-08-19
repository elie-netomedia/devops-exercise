pipeline {
    environment{
        registry = "elienetomedia/devops-exercise"
        regestryCredential = 'dockerhub-credentials	'
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
                }
                steps {
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
        }
}