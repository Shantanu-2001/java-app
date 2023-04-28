pipeline {

    agent any

    stages {

        stage("Git Checkout"){
            steps {
                git branch: 'main', url: 'https://github.com/Shantanu-2001/java-app.git'
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
        }

        stage("Quality Gate Analysis"){
            steps {
                script {
                   waitForQualityGate abortPipeline: false, credentialsId: 'f70fdf8b-aa63-4a47-b822-da8e98c621e8' 
                }
            }
        }
        stage("Upload Artifacts to Nexus"){
            steps {
                script {
                   nexusArtifactUploader artifacts:
                    [[
                    artifactId: 'springboot',
                    classifier: '',
                    file: 'target/UPES.jar',
                    type: 'jar'
                    ]], 
                    credentialsId: 'nexusid', 
                    groupId: 'com.example', 
                    nexusUrl: '43.204.232.117:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: 'Java_release', 
                    version: '1.0.0'
                }
            }
        }
    }
}
