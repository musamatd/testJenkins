pipeline {
  agent any
  stages {
    stage('Prepare') {
      steps {
        git(url: 'https://github.com/musamatd/testJenkins.git', branch: 'master')
      }
    }
    stage('Build Java') {
      parallel {
        stage('Build Java') {
          agent {
            docker {
              image 'maven:3-alpine'
              args '-v /root/.m2:/root/.m2'
            }

          }
          steps {
            sh 'mvn -B -DskipTests clean package'
          }
        }
        stage('Build Angular') {
          agent {
            docker {
              image 'node:6-alpine'
              args '-p 3000:3000'
            }

          }
          steps {
            sh 'npm install'
          }
        }
      }
    }
    stage('Test Java') {
      parallel {
        stage('Test Java') {
          steps {
            sh 'mvn test'
            junit 'target/surefire-reports/*.xml'
          }
        }
        stage('Test Angular') {
          steps {
            sh 'npm test'
          }
        }
      }
    }
    stage('Deploy') {
      steps {
        sh 'echo "deploying"'
      }
    }
  }
}