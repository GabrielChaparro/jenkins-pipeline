pipeline {
    agent any
    tools {
        maven "remote"
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -version'
                sh 'mvn clean install'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                dir("${WORKSPACE}"){
                // Run SonarQube analysis for Python
                script {
                    def scannerHome = tool name: 'sonar', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                    withSonarQubeEnv('sonar') {
                        sh "echo $pwd"
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
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