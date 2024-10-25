pipeline {

  environment {
    dockerimagename = "luckyehizefe1/my-app:1.1"
    dockerImage = ""
  }

  agent any

  stages {

    stage("Checkout source") {
      steps {
        // Check out from version control
        git 'https://github.com/luckyehizefe1/nodeapp_test.git/'
       }
    }

    stage("build image") {
      steps {
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }

      stage("pushing Image") {
        environment {
          registryCredential = 'dockerhublogin'
        }
      steps {
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential) {
            dockerImage.push("latest")
            }
          }
        }
      }

      stage("Deploying App to Kubernetes") {
        steps {
            script {
                kubernetesDeploy(configs: "deploymentservice.yaml", kubeconfigId: "kubernetes")
                }
            }
        }  
      }    
    }
