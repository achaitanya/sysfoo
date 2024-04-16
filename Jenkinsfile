pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        echo 'compiling the code'
        sh 'mvn compile'
      }
    }

    stage('test') {
      steps {
        echo 'running the unit tests'
        sh 'mvn clean test'
      }
    }

    stage('package') {
      steps {
        echo 'building the artifacts'
        sh 'mvn package -DskipTests'
      }
    }

    stage('sleep') {
      steps {
        sleep 5
      }
    }

    stage('Artifacts') {
      steps {
        archiveArtifacts 'target/*.war'
      }
    }

  }
  tools {
    maven 'Maven 3.6.3'
  }
  post {
    always {
      echo 'This pipeline is completed..'
    }

  }
}