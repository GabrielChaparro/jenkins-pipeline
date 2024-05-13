pipeline {
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
    }
   post {
        always {
            cleanWs()
        }
   }
}