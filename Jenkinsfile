pipeline {
  agent any

  tools {
    nodejs 'NodeJS-22.15.0'
  }

  stages {

    stage('Checkout') {
      steps {
        echo 'Checking out source code...'
        git 'https://github.com/shivashankargouda01/devfinal.git'
      }
    }

    stage('Install Dependencies') {
      steps {
        echo 'Installing backend dependencies...'
        dir('backend') {
          bat 'npm install'
        }

        echo 'Installing frontend dependencies...'
        dir('frontend') {
          bat 'npm install'
        }
      }
    }

    stage('Run Tests') {
      steps {
        echo 'Running backend tests...'
        dir('backend') {
          bat 'npm test -- --passWithNoTests'
        }

        echo 'Running frontend tests (non-blocking)...'
        dir('frontend') {
          script {
            try {
              bat 'npm test'
            } catch (Exception e) {
              echo 'Frontend tests failed or skipped.'
            }
          }
        }
      }
    }

    stage('Build Docker Images (for CD)') {
      steps {
        echo 'Building Docker images for deployment...'
        bat 'docker-compose build'
      }
    }

  }

  post {
    always {
      echo 'CI pipeline completed.'
    }
    failure {
      echo 'CI pipeline failed.'
    }
  }
}
