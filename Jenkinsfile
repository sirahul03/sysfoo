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
        sh 'mvn clean test'
      }
    }

    stage('package') {
      agent {
        docker {
          image 'maven:3.6.3-jdk-11-slim'
        }

      }
      when {
        branch 'master'
      }
      steps {
        sh 'mvn package -DskipTests'
        archiveArtifacts 'target/*.war'
      }
    }

    stage('Docker Build and Publish') {
      agent any
      when {
        branch 'master'
      }
      steps {
        script {
          docker.withRegistry('https://index.docker.io/v1/', 'dockerlogin') {

            def dockerImage = docker.build("sirahul033/sysfoo:v${env.BUILD_ID}", "./")

            dockerImage.push()

            dockerImage.push("latest")

          }
        }

      }
    }

    stage('Deploy and run') {
      agent any
      steps {
        sh 'docker-compose up -d'
      }
    }

  }
  tools {
    maven 'Maven 3.6.3'
  }
}