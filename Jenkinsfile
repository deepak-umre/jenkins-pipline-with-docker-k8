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
                    sh 'cp -r /var/lib/jenkins/workspace/deploy/target/*.war .'
                    def dockerImage = docker.build("deepakumre/tomcat1:latest")
                    withDockerRegistry([credentialsId: "dockerid", url: ""]) {
                        dockerImage.push()
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
}

