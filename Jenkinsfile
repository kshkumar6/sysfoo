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
        echo 'compile maven app'
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
        echo 'test maven app'
        sh 'mvn clean test'
      }
    }

    stage('package') {
      when {
         branch 'master'
      }
      parallel {
        stage ('create jar file') {
          agent {
            docker {
              image 'maven:3.6.3-jdk-11-slim'
            }
      }
      steps {
        echo 'package maven app'
        sh 'mvn package -DskipTests'
        archiveArtifacts '**/*.war'
      }
        }
      }
    }
    stage('OCI Image BnP') {
      when {
         branch 'master'
      }
      agent any
      steps {
        script {
          docker.withRegistry('https://index.docker.io/v1/', 'dockerlogin') {
            def dockerImage = docker.build("kshkumar6/sysfoo:v${env.BUILD_ID}", "./")
            dockerImage.push()
            dockerImage.push("latest")
            dockerImage.push("dev")
          }
        }

      }
    }
  }
  tools {
    maven 'maven 3.6.3'
  }
}
