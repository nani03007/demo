pipeline{
    agent any
    tools{
        maven 'MAVEN'
    }
    stages{
        stage('Checkout'){
            steps{
               checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/danvisrinivas/demo.git']])
        }
        }
        stage('Build maven project'){
            steps{
                bat "mvn clean install -DskipTests=true"
            }
        }
        stage('Test') {
            steps {
                script {
                    bat 'mvn test'
                }
            }
        }
    }
}
