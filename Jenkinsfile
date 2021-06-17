pipeline {

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git url:'https://github.com/heenapatel2/hellowhale.git', branch:'master'
      }
    }
    
      stage("Build image") {
            steps {
                script {
                    myapp = docker.build("heenapatel2986/hellowhale:${env.BUILD_ID}")
                }
            }
        }
    
      stage("Push image") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                            myapp.push("latest")
                            myapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }

    
    stage('Deploy App') {
      steps {
        script {
          kubeconfig(credentialsId: '4c999266-df02-45ec-88b4-c86f71e00fec', serverUrl: 'http://localhost:7777') {
          sh 'kubectl create -f $WORKSPACE/hellowhale.yml'
          sh 'kubectl get pods'
          sh 'kubectl get svc'
          echo 'open minikube ip and svc port which is 31113 in browser'
          echo 'deployment completed ........'
          kubernetesDeploy(configs: "hellowhale.yml", kubeconfigId: "mykubeconfig")
        }
      }
    }

  }

}
