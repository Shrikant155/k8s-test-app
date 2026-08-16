pipeline {
 agent any 

stages {
  stage("fetch-code") {
    steps {
     git branch: 'main',
     credentialsId: 'github-cred-id',
     url: 'https://github.com/Shrikant155/k8s-test-app.git'

    }
  }
  stage("deploy-to-minikube") {
    steps {
      sh '''
           minikube start --driver=docker
           minikube image load py-k8s-app:latest

          kubectl apply -f deployment.yml
          kubectl apply -f service.yml
          minikube service py-k8s-app-service
         '''
    }

  }

}
 post {
   success {
    mail to: "shrikantdevops999@gmail.com",
         subject: "successful :${JOB_NAME}",
         body: "jobname: ${JOB_NAME} buildno=${BUILD_NUMBER} url=${BUILD_URL}"

  }
  failure {
  echo "somethng is failed"
 }

 }


}
