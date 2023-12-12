pipeline {
    
    agent any 
    tools {
        maven 'maven'
    }
    
    stages {
        stage('Build maven') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/SindhooraDevops/devops-automation.git']])
                sh 'mvn clean install'
            }
        }
        
        stage('build docker image'){
            steps {
                script {
                    sh 'docker build -t sindhoorab/devops-integration .'
                }
            }
        }
        
        stage('push docker image') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpswd')]) {
                        sh 'docker login -u sindhoorab -p ${dockerhubpswd}'
                        sh 'docker push sindhoorab/devops-integration'         
}
                }
                
            }
        }
        
        stage ('deploy to kubernetes') {
            steps {
                script {
                    kubernetesDeploy (configs: 'deploymentservice.yaml',kubeconfigId: 'k8sconfigpwd')
                }
            }
        }
        
    }
}
