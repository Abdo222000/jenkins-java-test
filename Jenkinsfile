pipeline {
    agent {
        label 'agent1'
    }
    tools {
        maven 'maven-354'
        jdk 'jdk-11'
    }
    environment{
        DOCKER_USER = credentials('docker-username') // stored username in credentials
        DOCKER_PASS = credentials('docker-password') // stored password in credentials 
    }
    stages {
        stage("Build Java App"){
            steps{
                sh "java --version"
                sh "mvn package install"
            }
        }
        stage("Test java App"){
            steps{
                sh "mvn test"
            }
        }
        stage("Build Docker Image"){
            steps{
                sh "docker build -t ${DOCKER_USER}/iti-java:v${TAG_VERSION} ."
            }
        }
        stage("Docker Deploy"){
            steps{
                sh "docker run -d -p 8090:8090 --name iti-java ${DOCKER_USER}/iti-java:v${TAG_VERSION}"
            }
        }
    }
}