pipeline {
  agent { 
      docker { 
        image 'python:3.8'
        args '-u root:sudo'
      } 
    }
  stages {
    stage('build') {
      steps {
        sh 'pip install --user -r requirements.txt'
      }
    }
    stage('test') {
      steps {
        sh 'python test.py'
      }
      post {
        always {
          junit 'results.xml'
        }
      }    
    }
    stage('build and push') {
        steps {
            script {
                def image = docker.build("elienetomedia/devops-exercise:latest")
                /* Push the container to the custom Registry */
                image.push()
            }
        }
    }
  }
}