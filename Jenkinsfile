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
        stage('Pull Docker Image') {
            steps {
                script {
                    sh "docker login -u beratuyanik -p berat5745"
                    sh 'docker pull beratuyanik/helloworld:latest'
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