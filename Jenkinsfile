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
            string(credentialsId: 'kubernete-jenkins-server-account', variable: 'api_token')
        ]) {
            script {
                def deploymentExists = sh(script: 'kubectl --token $api_token --server http://192.168.56.1:8001 --insecure-skip-tls-verify=true get deployment deployment-coursemit-app-front-jenkins -o json', returnStatus: true)

                if (deploymentExists == 0) {
                    // Deployment exists, delete it
                    sh 'kubectl --token $api_token --server http://192.168.56.1:8001 --insecure-skip-tls-verify=true delete -f deployment-coursemit-app-front-jenkins.yaml'
                } else {
                    // Deployment does not exist, do nothing or perform alternative action
                    echo 'Deployment does not exist. Nothing to delete.'
                }
            }
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
