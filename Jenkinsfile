pipeline {

  agent any
  
  
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
       //         script {
       //             docker.withRegistry('https://registry.hub.docker.com', 'Docker_id') {
        //                    myapp.push("latest")
         //                   myapp.push()
          //                  //myapp.push("${env.BUILD_ID}")
            //        }
           // //    }
           // }
       // }
   
            stage('Push image') {
    withCredentials([usernamePassword( credentialsId: 'docker-hub-credentials', usernameVariable: 'dockerpractice101', passwordVariable: 'admin12345')]) {
        def registry_url = "registry.hub.docker.com/"
        bat "docker login -u $USER -p $PASSWORD ${registry_url}"
        docker.withRegistry("http://${registry_url}", "docker-hub-credentials") {
            // Push your image now
            bat "docker push username/foldername:build"
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
