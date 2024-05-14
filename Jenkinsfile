pipeline {
    agent any
    environment {
        GIT_COMMIT_SHORT = sh(
            script: "printf \$(git rev-parse --short ${GIT_COMMIT})",
            returnStdout: true
        )
    }
    tools {
        maven "remote"
    }
    stages {
        stage('Build Project') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('SonarQube Analysis') {
            environment {
                SCANNER_HOME = tool 'sonar-scanner'
            }
            steps {
                withSonarQubeEnv(credentialsId: 'jenkins', installationName: 'sonar') {
                    sh '''$SCANNER_HOME/bin/sonar-scanner \
                    -Dsonar.projectKey=projectKey \
                    -Dsonar.projectName=projectName \
                    -Dsonar.sources=src/ \
                    -Dsonar.java.binaries=target/classes/ \
                    -Dsonar.exclusions=src/test/java/****/*.java \
                    -Dsonar.java.libraries=/var/lib/jenkins/.m2/**/*.jar \
                    -Dsonar.projectVersion=${BUILD_NUMBER}-${GIT_COMMIT_SHORT}'''
                }
            }
        }
        stage('SQuality Gate') {
            steps {
            timeout(time: 1, unit: 'MINUTES') {
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