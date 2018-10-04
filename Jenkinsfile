pipeline {
  agent none
  stages {
    stage('compilar') {
      parallel {
        stage('compilar') {
          agent {
            node {
              label 'ubuntu'
            }

          }
          steps {
            sh 'mvn clean package'
            archiveArtifacts(artifacts: '**/*.war', fingerprint: true, onlyIfSuccessful: true)
          }
        }
        stage('checkstyle') {
          agent {
            node {
              label 'ubuntu'
            }

          }
          steps {
            sh 'mvn checkstyle:checkstyle'
            checkstyle()
          }
        }
      }
    }
    stage('aprobar') {
      steps {
        input(message: 'aprobar despliegue', ok: 'SI')
      }
    }
    stage('despliegue') {
      agent {
        node {
          label 'Windows'
        }

      }
      steps {
        copyArtifacts(projectName: 'maven-project', fingerprintArtifacts: true, flatten: true)
      }
    }
  }
  environment {
    TOMCAT_HOME = 'C:\\Tomcat_8.5\\webapps'
  }
}