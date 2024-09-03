pipeline {
    agent any
    tools {
        maven 'maven3'
        jdk 'jdk17'
    }
    environment {
        SCANNER_HOME= tool 'sonar-scanner'
}

    stages {
        stage('gitcheckout') {
            steps {
                git branch: 'main', credentialsId: 'git-cred', url: 'https://github.com/sj27773/FullStack-Blogging-App.git'
            }
        }

        stage('Compile') {
            steps {
                sh "mvn compile"
            }
        }
        
        stage('Test') {
            steps {
                sh "mvn test"
            }
        }
    }
}

