pipeline {

    agent any

    stages {

        stage("Git Checkout"){
            steps {
                git branch: 'main', url: 'https://github.com/Shantanu-2001/java-app'
            }
        }

        stage("Unit Testing"){
            steps {
                sh 'mvn test'
            }
        }

        stage("Integration Testing"){
            steps {
                sh 'mvn verify -DskipUnitTests'
            }
        }

        stage("Build"){
            steps {
                sh 'mvn clean install'
            }
        }

        stage("Static Code Analysis"){
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'f70fdf8b-aa63-4a47-b822-da8e98c621e8') {
                        sh 'mvn clean package sonar:sonar'
                    }
                }
            }
            
            stage("Quality Gate Analysis"){
            steps {
                script {
                   waitForQualityGate abortPipeline: false, credentialsId: 'f70fdf8b-aa63-4a47-b822-da8e98c621e8' 
                }
            }
        }
        }
    }
}
