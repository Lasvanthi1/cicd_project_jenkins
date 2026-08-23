pipeline {
    agent any
    stages{
        stage('Build Maven'){
            steps{
                git credentialsId: 'githubcred', url: 'https://github.com/Lasvanthi1/cicd_project_jenkins.git'
               sh 'mvn clean install'
               sh 'mvn clean package'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t lasvanthi/cicdjavaproject:v1 .'
                }
            }
        }
          stage('Docker login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhubcred', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh 'docker push lasvanthi/cicdjavaproject:v1'
                }
            }
        }
        
        
        stage('Deploy to k8s'){

            steps{
                script{
                     kubernetesDeploy (configs: 'deploymentservice.yaml' ,kubeconfigId: 'k8sconfigpwd')
                   
                }
            }
        }
    }
}
