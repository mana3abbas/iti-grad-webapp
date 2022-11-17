pipeline {
  agent { label "final-project"}
  stages {
    stage('build') {
      steps {
        script {
       
            withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'username', passwordVariable: 'password')]) {
              sh """
        
                  docker login -u ${username} -p ${password}
                  docker build -t monasamir/final .
                  docker push monasamir/final
              """
          }
        } 
      }
    }
    stage('deploy') {
    
      steps {
        script {

             withCredentials([file(credentialsId: 'key', variable: 'key')]) {
              sh """
                  gcloud auth activate-service-account  my-first-service-account@monas-project-367813.iam.gserviceaccount.com --key-file ${key}
        
                """
            }
          
        }


            withCredentials([file(credentialsId: 'kube', variable: 'KUBECONFIG')]) {
              sh """
                  
                  kubectl apply -f Deployment --kubeconfig=${KUBECONFIG}
                """
            }
        }
      }
    }
  }
}
