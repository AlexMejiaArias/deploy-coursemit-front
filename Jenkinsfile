pipeline {
  agent any
  stages {
    
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
   post {
        success {
            script {
                slackSend(channel: '#jenkins', color: 'good', message: "El pipeline ha finalizado exitosamente")
            }
        }
        failure {
            script {
                slackSend(channel: '#jenkins', color: 'danger', message: "El pipeline ha fallado")
            }
        }
    }
  }
