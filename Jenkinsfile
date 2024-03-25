pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Get some code from a GitHub repository
                git branch: 'develop', credentialsId: 'gitcred', url: 'https://github.com/surajsdevops/automation-to-k8s.git'

            }
        }
         stage('Maven build') {
            
            steps {
                
                script{
                    
                    sh 'mvn clean install'
                }
            }
        }
        stage ('Docker Build Stage') {
            steps {
                script {
                    sh 'docker build -t $JOB_NAME:v1.$BUILD_ID .'
                    sh 'docker tag $JOB_NAME:v1.$BUILD_ID surajsdevops/$JOB_NAME:v1.$BUILD_ID'
                }
            }
        }
        stage ('push docker image to  dockerhub') {
            steps {
                script {
		   //withCredentials([usernamePassword(credentialsId: 'dockerhub-cred', passwordVariable: 'dockerhub-pwd', usernameVariable: 'dockerhub-uname'), 
                   withCredentials([usernamePassword(credentialsId: 'dockerhub-cred', passwordVariable: 'dockerhub-pwd', usernameVariable: 'dockerhub-uname')]) {
                        sh 'docker login -u surajsdevops -p $dockerhub-pwd'
                        sh 'docker push surajsdevops/$JOB_NAME:v1.$BUILD_ID'
                        sh "docker rmi surajsdevops/$JOB_NAME:v1.$BUILD_ID"
                        sh 'docker run  -d -p 9292:8080 surajsdevops/$JOB_NAME:v1.$BUILD_ID'
                        }
                    }
                }
        }
    }
}


