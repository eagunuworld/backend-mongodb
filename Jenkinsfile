pipeline{
    agent any

       environment {
            DEPLOY = "${env.BRANCH_NAME == "development" || env.BRANCH_NAME == "master" ? "true" : "false"}"
            NAME = "${env.BRANCH_NAME == "master" ? "example" : "examp-staging"}"
            def mavenHome =  tool name: "maven:3.8.5", type: "maven"
            VERSION = "${env.BUILD_ID}"
            REGISTRY = 'eagunuworld/eagunu-mongo-db'
            REGISTRY_CREDENTIAL = 'eagunuworld_credentials'
          }

    stages {
      stage('PersistentVolume And PersistentVolumeClaim') {
              steps {
                  withCredentials([kubeconfigFile(credentialsId: 'my-configurations', variable: 'KUBECONFIG')]) {
                    sh "kubectl apply -f pv_pvc_manual.yml"
                        }
                      }
                 }

        stage('Deploy MongoDB In Kubernetes Clusters') {
               steps {
                   withCredentials([kubeconfigFile(credentialsId: 'my-configurations', variable: 'KUBECONFIG')]) {
                   sh "helm upgrade --install  --force --set name=${NAME} mongodbapp mongo/"
                     }
                 }
             }

        stage('Helm Version Deployment Releases') {
                  steps {
                      withCredentials([kubeconfigFile(credentialsId: 'my-configurations', variable: 'KUBECONFIG')]) {
                      sh "helm list --date"
                    }
              }
          }
    }
}
