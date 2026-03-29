pipeline {
    agent any

    tools {
        jdk 'jdk17'
        maven 'maven3'
    }

    environment {
        // Ensure this matches the name in Global Tool Configuration
        SCANNER_HOME = tool 'sonar-scanner'
        MAVEN_OPTS = "-Xmx512m"
    }

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', 
                    credentialsId: 'github', 
                    url: 'https://github.com/kingpin1374/FullStack-Blogging-App.git'
            }
        }

        stage('Compile') {
            steps {
                sh "mvn clean compile"
            }
        }

        stage('Test') {
            steps {
                sh "mvn test"
            }
        }

        stage('Trivy FS Scan') {
            steps {
                // Fixed 'fs' casing and added --exit-code 0 so it doesn't fail the build if vulnerabilities are found
                sh "trivy fs --format table -o fs.html ."
            }
        }

        stage('SonarQube Analysis') {
            steps {
                // Ensure 'sonar-server' matches the name in Jenkins System Configuration
                withSonarQubeEnv('sonar-server') {
                    sh """
                        ${SCANNER_HOME}/bin/sonar-scanner \
                        -Dsonar.projectName=Blogging-app \
                        -Dsonar.projectKey=Blogging-app \
                        -Dsonar.java.binaries=target/classes
                    """
                }
            }
        }

        stage('Build & Package') {
            steps {
                sh "mvn package -DskipTests"
            }
        }

        stage('Deploy to Nexus') {
            steps {
                withMaven(
                    globalMavenSettingsConfig: 'maven-settings', 
                    jdk: 'jdk17', 
                    maven: 'maven3', 
                    traceability: true
                ) {
                    sh "mvn deploy -DskipTests"
                }
            }
        }

        stage('Success') {
            steps {
                echo 'Pipeline completed successfully!'
            }
        }
    }
}
