pipeline {
    agent any
    stages {
        stage('Pull') {
            steps {
                echo "Successful pull from Git"
                git 'https://github.com/deepak-umre/jenkins-pipline-with-docker-k8.git'
            }
        }
        stage('Build') {
            steps {
                echo "Building with Maven"
                sh 'mvn clean package'
            }
        }
        stage('creating tomcat image Tomcat') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: '0ae5f4c0-7981-4c07-a9d8-b35ee82c8029', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {

                    sh '''cp -r /var/lib/jenkins/workspace/deploy/target/*.war .
                    docker login
                    docker build -t deepakumre/tomcat1 . 
                    docker push deepakumre/tomcat1'''
                    }
                }
            }
        }
        stage('build image on k8 ') {
            steps {
                script {
                    sh 'kubectl apply -f deployment.yaml'
                }
            }
        }
        stage('getting info') {
            steps {
                script {
                    sh '''kubectl get pods -o wide 
                    kubectl get nodes -o wide 
                    kubectl get svc -o wide 
                    ls /var/lib/jenkins/workspace/deploy/target/'''
                }
            }
        }
    }
} //update
