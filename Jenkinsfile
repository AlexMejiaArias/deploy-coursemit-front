pipeline {
  agent any
  stages {
    stage('clone repository') {
      steps {
        sh '''java -version
mvn --version
git --version'''
      }
    }
      stage('Delete existing deployment') {
            steps {
                withCredentials(bindings: [
                    string(credentialsId: 'kubernete-jenkis-server-account', variable: 'api_token')
                ]) {
                    sh 'kubectl --token $api_token --server http://192.168.56.1:8001 --insecure-skip-tls-verify=true delete -f deployment-coursemit-app-front-jenkins.yaml '
                }
            }
        }


    stage('Deploy billing App') {
      steps {
        withCredentials(bindings: [
                      string(credentialsId: 'kubernete-jenkis-server-account', variable: 'api_token')
                      ]) {
            sh 'kubectl --token $api_token --server http://192.168.56.1:8001 --insecure-skip-tls-verify=true apply -f deployment-coursemit-app-front-jenkins.yaml '
          }

        }
      }

    }
  }