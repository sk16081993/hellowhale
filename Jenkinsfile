pipeline {

  agent any
  environment {
        //once you sign up for Docker hub, use that user_id here
        registry = "dockerpractice101/hellowhale"
        //- update your credentials ID after creating credentials for connecting to Docker Hub
        registryCredential = 'dockerhub_id'
   }
  
  stages {

    stage('Checkout Source') {
      steps {
        git url:'https://github.com/sk16081993/hellowhale.git', branch:'master'
      }
    }
    
      stage("Build image") {
            steps {
                script {
                    myapp = docker.build("sk16081993/hellowhale:${env.BUILD_ID}")
                }
            }
        }
    
     // stage("Push image") {
     //       steps {
     //           script {
      //              docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
        //                    myapp.push("latest")
         //                   myapp.push("${env.BUILD_ID}")
         //           }
                }
         //   }
      //  }
    stage("Push image") {
            steps {
                script {
                    docker.withRegistry('', 'registryCredential') {
                            myapp.push("latest")
                            myapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }

    
    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "hellowhale.yml", kubeconfigId: "mykubeconfig")
        }
      }
    }

  }

}
