pipeline {
    agent any
    tools {
        maven 'maven3'
        jdk 'jdk17'
    }
    environment {
        SCANNER_HOME = tool 'sonar-scanner'
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

        stage('Trivy FS Scan') {
            steps {
                sh 'trivy fs --format table -o fs.html .'
            }
        }
        
        stage('Sonar Analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh '''
                    $SCANNER_HOME/bin/sonar-scanner \
                    -Dsonar.projectName=taskmaster \
                    -Dsonar.projectKey=taskmaster \
                    -Dsonar.java.binaries=target
                    '''
                }
            }
        }

        stage('Docker Build & Tag') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh 'docker build -t sj27773/taskmaster:latest .'
                    }   
                }
            }
        }
    }
}
