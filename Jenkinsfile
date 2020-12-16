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
       sh "rm -rf truffle.json || true"
       sh "docker run dxa4481/trufflehog:latest --json https://github.com/toshith/devsecops.git > truffle.json"
       sh "cat truffle.json"
      }
   }
   stage('SCA'){
	    steps {
		      sh "pip3 install safety"
		      sh "rm -rf safety.json || true"
		      sh "safety check -r requirements.txt --json > safety.json"
		      sh "cat safety.json"
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
   
