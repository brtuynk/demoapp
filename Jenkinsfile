pipeline {
    agent any
    stages {
        stage('Checkout GitHub Source') {
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'github', url: 'https://github.com/brtuynk/demoapp.git']]])
            }
        }
        stage("Build Project") {
            steps {
                echo 'cleaning, compiling, and installing project...'
                sh "./mvnw clean install"
            }
        }
        stage('Run Tests') {
            steps {
                echo 'running tests...'
                sh "./mvnw test"
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh ' docker build -t beratuyanik/helloworld:latest .'
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'pwd', usernameVariable: 'uname')]) {
                        sh "docker login -u $uname -p $pwd"
                        sh 'docker push beratuyanik/helloworld:latest'
                    }
                }
            }
        }
        stage('Deploy App on K8S') {
            agent {
                label deploy 
            }
            
            steps {
                sh 'kubectl apply -f myweb.yaml'
                }
        }
    }
}