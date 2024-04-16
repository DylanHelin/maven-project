pipeline {
    agent any
    
    tools {
        maven 'localMaven'
    }
    
    stages {
        stage('Clone') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name : '*/master']],
                    extensions : [],
                    userRemoteConfigs: [[
                        url: 'https://github.com/DylanHelin/maven-project.git', 
                        credentialsId: 'sucret_key_4_github'
                    ]]    
                ])
            }
        }
        
        stage('Compile') {
            steps {
                withMaven(maven: 'localMaven') {
                    bat "mvn compile"
                }
            }
        }
        
        stage('Test') {
            steps {
                withMaven(maven: 'localMaven') {
                    bat "mvn test"
                }
            }
        }
        
        stage('Build') {
            steps {
                withMaven(maven: 'localMaven') {
                    bat 'mvn -B -DSkipTests clean package'
                }
            }
        }
        
        stage('Build and send Results Sonar') {
            steps {
                withSonarQubeEnv(
                    installationName: 'sonar',
                    credentialsId: 'sucret4sonar'
                ) {
                    bat 'mvn -B -DSkipTests clean package sonar:sonar'
                }
            }
        }
    }
}
