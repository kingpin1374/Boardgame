pipeline {
    agent any

    tools {
        jdk 'jdk17'
        maven 'maven3'
    }

    environment {
        // Points to the SonarQube Scanner tool configured in Jenkins
        SCANNER_HOME = tool 'sonar-scanner'
    }

    stages {
        stage('Git Checkout') {
            steps {
                git credentialsId: 'github', url: 'https://github.com/kingpin1374/Boardgame.git'
            }
        }

        stage('Compilation') {
            steps {
                sh "mvn clean compile"
            }
        }

        stage('Test') {
            steps {
                sh "mvn test"
            }
        }

        stage('File System Scan (Trivy)') {
            steps {
                // Scans the source code for vulnerabilities before building the JAR
                sh "trivy fs --format table -o trivy-fs-report.html ."
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh """${SCANNER_HOME}/bin/sonar-scanner \
                        -Dsonar.projectName=BoardGame \
                        -Dsonar.projectKey=BoardGame \
                        -Dsonar.java.binaries=. """
                }
            }
        }

        stage('Quality Gate') {
            steps {
                script {
                    // Pipeline will wait for SonarQube to finish its background task
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-token'
                }
            }
        }

        stage('Build Artifact') {
            steps {
                sh 'mvn package -DskipTests'
            }
        }

        stage('Publish to Nexus') {
            steps {
                // Uses the global-settings.xml we fixed to authenticate with Nexus
                withMaven(globalMavenSettingsConfig: 'global-settings', jdk: 'jdk17', maven: 'maven3', traceability: true) {
                    sh "mvn deploy -DskipTests"
                }
            }
        }

        stage('Docker Build & Tag') {
            steps {
                script {
                    // Removed 'toolName' to use the system's modern Docker client
                    withDockerRegistry(credentialsId: 'docker-cred', url: 'https://index.docker.io/v1/') {
                        sh "docker build -t amandevops001/boardshack:latest ."
                    }
                }
            }
        }

        stage('Docker Image Scan (Trivy)') {
            steps {
                sh "trivy image --format table amandevops001/boardshack:latest"
            }
        }

        stage('Docker Push') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', url: 'https://index.docker.io/v1/') {
                        sh "docker push amandevops001/boardshack:latest"
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                withKubeConfig(caCertificate: '', clusterName: 'kubernetes', contextName: '', credentialsId: 'k8-cred', namespace: 'webapps', restrictKubeConfigAccess: false, serverUrl: 'https://172.31.39.214:6443') {
                    sh "kubectl apply -f deployservice.yml"
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                withKubeConfig(caCertificate: '', clusterName: 'kubernetes', contextName: '', credentialsId: 'k8-cred', namespace: 'webapps', restrictKubeConfigAccess: false, serverUrl: 'https://172.31.39.214:6443') {
                    sh "kubectl get pods -n webapps"
                    sh "kubectl get svc -n webapps"
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline Execution Finished.'
        }
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo 'Pipeline Failed. Please check the logs.'
        }
    }
}
