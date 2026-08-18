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
/*  stage("deploy-to-minikube") {
    steps {
      sh '''
           minikube delete || true
           minikube start --driver=docker
           minikube image load py-k8s-app:latest
          kubectl apply -f k8s-ymls/namespaces.yml

          kubectl apply -f k8s-ymls/dev/deployment.yml
          kubectl apply -f k8s-ymls/dev/service.yml
          kubectl apply -f k8s-ymls/prod/deployment.yml
          kubectl apply -f k8s-ymls/prod/service.yml

  kubectl apply -f k8s-ymls/staging/deployment.yml
          kubectl apply -f k8s-ymls/staging/service.yml


 kubectl apply -f k8s-ymls/qa/deployment.yml
          kubectl apply -f k8s-ymls/qa/service.yml

            kubectl wait --for=condition=ready pod -l app=py-k8s-app --timeout=240s
         minikube service py-k8s-app-service --url
         '''
    }

  }*/
  stage("minikube start") {
   steps {
    sh '''
           minikube delete || true
           minikube start --driver=docker
          
         #minikube status || minikube start --driver=docker
        # minikube image load  py-k8s-app:latest
       '''
   }
  }

   stage("createnamespace")  {
      steps {
        sh ' kubectl apply -f k8s-ymls/namespaces.yml' 
      }
   }
   stage("dev-deploy") {
     steps {
     sh '''
       kubectl apply -f k8s-ymls/dev/deployment.yml
          kubectl apply -f k8s-ymls/dev/service.yml
         kubectl wait --for=condition=ready pod -l app=py-k8s-app -n dev --timeout=240s
        '''

     }
   }
stage("prod-deploy") {
     steps {
     sh '''
       kubectl apply -f k8s-ymls/prod/deployment.yml
          kubectl apply -f k8s-ymls/prod/service.yml
      kubectl wait --for=condition=ready pod -l app=py-k8s-app -n prod --timeout=240s
        '''

     }
   }
    stage("staging-deploy") {
     steps {
     sh '''
       kubectl apply -f k8s-ymls/staging/deployment.yml
          kubectl apply -f k8s-ymls/staging/service.yml
          kubectl wait --for=condition=ready pod -l app=py-k8s-app -n staging --timeout=240s
        '''

     }
   }

   stage("qa-deploy") {
     steps {
     sh '''
       kubectl apply -f k8s-ymls/qa/deployment.yml
          kubectl apply -f k8s-ymls/qa/service.yml
        kubectl wait --for=condition=ready pod -l app=py-k8s-app -n qa --timeout=240s
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
