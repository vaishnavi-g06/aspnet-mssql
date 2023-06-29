pipeline {
   agent any
   
    stages {

        stage ('checkout') {
            steps {
            checkout([$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/vaishnavi-g06/aspnet-mssql']]])
            }
        }
       
        stage ('Build docker image') {
            steps {
               
                dockerImage = docker.build registry
                
            }
        }
       
         // Uploading Docker images into Docker Hub
    stage('Upload Image') {
     steps{   
        
            withDockerRegistry([ credentialsId: "dockerhub", url: "" ]) {
        dockerImage.push()
        
            }
        }
      }
    
   
    stage ('K8S Deploy') {
        steps {
           
                kubernetesDeploy(
                    configs: 'k8s-deployment.yaml',
                    kubeconfigId: 'K8S',
                    enableConfigSubstitution: true
                    )           
               
            
        }
    }
  
    }  
}
