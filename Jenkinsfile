pipeline {
    agent any
    tools {
        maven "remote"
    }
    stages {
        stage('Checkout') {
        stage('Build') {
            steps {
                sh 'mvn -version'
                sh 'mvn clean install'
            }
        }
                stage('SonarQube Analysis') {
                    steps {
                        // Run SonarQube scanner
                        withSonarQubeEnv('sonar') {
                            sh 'mvn sonar:sonar'
                        }
                    }
                }

                stage('Quality Gate') {
                    steps {
                        // Wait for SonarQube analysis to complete and check the quality gate
                        timeout(time: 1, unit: 'HOURS') {
                            waitForQualityGate abortPipeline: true
                        }
                    }
                }
    }
   post {
        always {
            cleanWs()
        }
   }
}