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
        stage('Git Checkout') {
            steps {
                git branch: 'main', credentialsId: 'git-cred', url: 'https://github.com/sj27773/FullStack-Blogging-App.git'
            }
        }
        stage('Compile') {
            steps {
                sh "mvn compile"
            }
        }
        stage('Package') {
            steps {
                sh "mvn package"
            }
        }
        stage('Sonar Analysis') {
            steps {
                withSonarQubeEnv('sonar-scanner') {
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
        stage('K8 Deploy') {
            steps {
                withKubeConfig(caCertificate: '', clusterName: 'devopsshack-cluster',
                       contextName: '', credentialsId: 'k8-cred', namespace: 'webapps',
                       restrictKubeConfigAccess: false, serverUrl:
                       'https://0B4099E78C18E7E0A52C1F3D8EFCAFE0.gr7.us-east-1.eks.amazonaws.com') {
            sh 'kubectl apply -f deployment-service.yml'
            sleep 30
        }
    }
}


