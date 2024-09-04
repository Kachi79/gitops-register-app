pipeline {
  agent { label 'Jenkins-Agent'}
  environment {
            APP_NAME = "register-app-pipeline"
  }

   stages {
      stage("Cleanup Workspace") {
        steps {
          cleanWs()
       }
     }
  stage('Checkout From Git') {
     steps {
          git branch: 'main', credentialsId: 'GitHub-Token', url: 'https://github.com/Kachi79/gitops-register-app.git' 
        }
      }
  stage('Update The Deployments Tags') {
     steps {
         sh """ 
             cat deployment.yaml 
             sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
             cat deployment.yaml
           """  
        }
      }
 stages {
      stage("Push The Changed Deployment File to Git") {
        steps {
            sh """
                git config --global user.name "kachi79"
                git config --global user.email "niccolobussoti@yahoo.com"
                git add deployment.yaml
                git commit -m "Updated Deployment Manifest"
            """   
            withCredentials([gitUsernamePassword(credentialsId: 'GitHub-Token', gitToolName: 'Default')]) {
                sh "git push https://github.com/Kachi79/gitops-register-app main"
             }
        }   
     }    
   }
}
