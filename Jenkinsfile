@Library('abdo.java') _
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
                script{
                    def docker_java = new abdo.java.docker()
                    docker_java.print_java_version()
                    docker_java.install_packages()
                }
            }
        }
        stage("Test java App"){
            steps{
                script{
                    def docker_java = new abdo.java.docker()
                    docker_java.test_app()
                }
            }
        }
        stage("Build Docker Image"){
            steps{
                script{
                    def docker_java = new abdo.java.docker()
                    docker_java.build("${DOCKER_USER}/iti-java","v${TAG_VERSION}")
                }
            }
        }
        stage("Docker Deploy"){
            steps{
                script{
                    def docker_java = new abdo.java.docker()
                    docker_java.run("${DOCKER_USER}/iti-java","v${TAG_VERSION}")
                }
            }
        }
    }
}