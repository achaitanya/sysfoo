pipeline {
  agent none
  stages {
    stage('build') {
      agent {
        docker {
          image 'maven:3.6.3-jdk-11-slim'
        }

      }
      steps {
        echo 'compiling the code'
        sh 'mvn compile'
      }
    }

    stage('test') {
      agent {
        docker {
          image 'maven:3.6.3-jdk-11-slim'
        }

      }
      steps {
        echo 'running the unit tests'
        sh 'mvn clean test'
      }
    }

    stage('package') {
      parallel {
        stage('package') {
          agent {
            docker {
              image 'maven:3.6.3-jdk-11-slim'
            }

          }
          steps {
            echo 'building the artifacts'
            sh 'mvn package -DskipTests'
          }
        }

        stage('Docker Build & Publish') {
          agent any
          steps {
            script {
              docker.withRegistry('https://index.docker.io/v1/', 'dockerlogin') {
                def dockerImage = docker.build("amar0203/sysfoo:v${env.BUILD_ID}", "./")
                dockerImage.push()
                dockerImage.push("latest")
                dockerImage.push("dev")
              }
            }

          }
        }

      }
    }

  }
  post {
    always {
      echo 'This pipeline is completed..'
    }

  }
}