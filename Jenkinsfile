pipeline {
 environment{
    registry= "toshith/dev"
    registryCredential = "DockerHub"
    dockerImage = ''
  }
 agent any
 
 stages {
   stage('Check for secrets'){
     steps {
       sh "rm -rf trufflehog.json || true"
       sh "docker run dxa4481/trufflehog:latest --json https://github.com/toshith/CyberFRAT-DevSecOps-Training-Sample-Flask-App.git --max_depth=1 > trufflehog.json"
       sh "cat trufflehog.json"
      }
   }
   stage('Build docker image') {
     steps {
      script {
            dockerImage= docker.build registry + ":$BUILD_NUMBER"
          }       
        }
      }
  stage ('Push to docker Hub') {
   steps {
    script {
         docker.withRegistry('', registryCredential ) {
          dockerImage.push()
         }
       }
     }
  }
       
   stage('Test Run') {
      steps {
        sh 'docker run -d $registry:$BUILD_NUMBER'          
          }
        }
      }
   }
   
