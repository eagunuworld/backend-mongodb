pipeline{
    agent any

       environment {
            DEPLOY = "${env.BRANCH_NAME == "development" || env.BRANCH_NAME == "master" ? "true" : "false"}"
            NAME = "${env.BRANCH_NAME == "master" ? "example" : "mongo"}"
            def mavenHome =  tool name: "maven:3.8.5", type: "maven"
            VERSION = "${env.BUILD_ID}"
            REGISTRY = 'eagunuworld/eagunu-mongo-db'
            REGISTRY_CREDENTIAL = 'eagunuworld_credentials'
          }

    stages {
      stage('PersistentVolume And PersistentVolumeClaim') {
              steps {
                  withCredentials([kubeconfigFile(credentialsId: 'my-configurations', variable: 'KUBECONFIG')]) {
                    sh "kubectl apply -f pv_pvc_4_rs.yml"
                        }
                      }
                 }

   stage('kubectl Get PV,PVC') {
              steps {
                  withCredentials([kubeconfigFile(credentialsId: 'my-configurations', variable: 'KUBECONFIG')]) {
                      sh "kubectl get pv,pvc -A"
                        }
                      }
                  }

        stage('Deploy MongoDB In Kubernetes Clusters') {
               steps {
                   withCredentials([kubeconfigFile(credentialsId: 'my-configurations', variable: 'KUBECONFIG')]) {
                   sh "kubectl apply -f db-replicatSet.yml"
                     }
                 }
             }

      // stage('Deploy MongoDB In K8s As StatefulSet') {
      //        steps {
      //               withCredentials([kubeconfigFile(credentialsId: 'my-configurations', variable: 'KUBECONFIG')]) {
      //                   sh "kubectl apply -f db-statefulSet.yml"
      //                     }
      //                 }
      //             }

      stage('Deploy MongoDB Service') {
             steps {
                 withCredentials([kubeconfigFile(credentialsId: 'my-configurations', variable: 'KUBECONFIG')]) {
                 sh "kubectl apply -f mongodb-service.yml"
                   }
               }
           }

      stage('kubectl get po,svc') {
              steps {
                  withCredentials([kubeconfigFile(credentialsId: 'my-configurations', variable: 'KUBECONFIG')]) {
                      sh "kubectl get po,svc -A"
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
