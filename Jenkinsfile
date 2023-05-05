pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        echo 'compile maven app'
        sh 'mvn compile'
      }
    }

    stage('test') {
      parallel{
        stage ('path 1') {
          steps {
              echo 'test maven app'
              sh 'mvn clean test'
                }
        }
        stage ('path 2') {
          steps {
            echo 'parallel step 1'
            sleep 2
            }
         }
    }
    }

    stage('package') {
      steps {
        echo 'package maven app'
        sh 'mvn package -DskipTests'
        archiveArtifacts '**/*.war'
      }
    }

  }
  tools {
    maven 'maven 3.6.3'
  }
}
