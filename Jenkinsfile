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
    stage('test-fail') {
        steps {
            echo "do you run?"
        }
    }
  }
}