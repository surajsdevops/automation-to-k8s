pipeline {
    
    agent any 
    
    stages {
        stage('Git Checkout'){
            steps{
                script{
                    git branch: 'develop', url: 'https://github.com/khadar099/automation-to-k8s.git'
                    }
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
                    sh 'docker image build -t $JOB_NAME:v1.$BUILD_ID .'
                    sh 'docker image tag $JOB_NAME:v1.$BUILD_ID khadar3099/$JOB_NAME:v1.$BUILD_ID'
                }
            }
        }
        stage ('push docker image to  dockerhub') {
            steps {
                script {
                   withCredentials([string(credentialsId: 'dockerhub-token', variable: 'dockerhub_psd')]) {
                        sh 'docker login -u khadar3099 -p ${dockerhub_psd}'
                        sh 'docker image push khadar3099/$JOB_NAME:v1.$BUILD_ID'
                        sh "docker rmi khadar3099/$JOB_NAME:v1.$BUILD_ID"
                        sh 'docker run  -d -p 9292:8080 khadar3099/$JOB_NAME:v1.$BUILD_ID'
                        }
                    }
                }
            }
    }
}
