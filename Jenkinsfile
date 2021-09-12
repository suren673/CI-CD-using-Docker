/*pipeline {
    agent any
	
	  tools
    {
       maven "MAVEN_HOME"
    }*/
pipeline {
	agent any
	
	  tools
    {
       maven "MAVEN_HOME"
    }
environment {
registry = "suren67/samplewebapp"
registryCredential = 'dockerhub_id'
dockerImage = ''
	

//PROJECT_ID = 'true-campus-320305'
 //GCLOUD_K8S_CLUSTER_NAME = 'gkejenkincluster'
  LOCATION = 'us-central1-c'
  //JENKINSGCLOUDCREDENTIAL = 'Jenkin_GCP_Cred_ID'
   
	
}
 stages {
      stage('Checkout code') {
           steps {
             
                git branch: 'master', url: 'https://github.com/suren673/CI-CD-using-Docker.git'
             
          }
        }
	 stage('Execute Maven') {
           steps {
             
                sh 'mvn package'             
          }
        }
        

  stage('Docker Build and Tag') {
           steps {
              script {
		dockerImage = docker.build registry + ":$BUILD_NUMBER"
		}
                //sh 'docker build -t samplewebapp:latest .' 
                //sh 'docker tag samplewebapp suren67/samplewebapp:latest'
                //sh 'docker tag samplewebapp nikhilnidhi/samplewebapp:$BUILD_NUMBER'
               
          }
        }
     
  stage('Publish image to Docker Hub') {
          
            steps {
        /*withDockerRegistry([ credentialsId: "dockerhub_id", url: "" ]) {
          sh  'docker push suren67/samplewebapp:latest'
            }*/
		    script {
docker.withRegistry( '', registryCredential ) {
dockerImage.push()
}
}
                  
          }
        }
	 
stage('Deploy to GKE') {
            /*steps{
                sh "sed -i 's/samplewebapp:latest/samplewebapp:${env.BUILD_ID}/g' deployment.yaml"
                step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, 
		      location: env.LOCATION, manifestPattern: 'deployment.yaml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
            }*/
	steps{
    withCredentials([file(credentialsId: "${JENKINS_GCLOUD_CRED_ID}", variable: 'JENKINSGCLOUDCREDENTIAL')])
    {
    sh """
        gcloud auth activate-service-account --key-file=${true-campus-320305}
        gcloud config set compute/zone us-central1-c
        gcloud config set compute/region us-central1
        gcloud config set project true-campus-320305
        gcloud container clusters get-credentials gkejenkincluster

        }	 
	 }
     }
     
    /*  stage('Run Docker container on Jenkins Agent') {
             
            steps 
			{
				
                sh 'docker run -d -p 8004:8080 suren67/samplewebapp'
 
            }
        }
 stage('Run Docker container on remote hosts') {
             
            steps {
                sh "docker -H ssh://jenkins@172.31.28.25 run -d -p 8003:8080 nikhilnidhi/samplewebapp"
 
            }
        }*/
    }
	}
    
