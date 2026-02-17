pipeline{
    agent any
    tools{
        maven 'MAVEN'
    }
    environment{
        APP_DIR = "/opt/springboot-app"
        JAR_NAME = "app.jar"
        BUILD_JAR = "target/demo-0.0.3-SNAPSHOT.jar"
        IMAGE="veera03007/springboot-app"
        USER_NAME=credentials('dockerhub')."veera03007"
        PASSWORD=credentials('dockerhub')."Naruto@7019"

    }
    stages{
        stage('Checkout'){
            steps{
               checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/nani03007/demo.git']])
        }
        }
    }
        stage('Build maven project'){
            steps{
                sh "mvn clean install"
            }
        }
        stage('Test') {
            steps {
                script {
                    sh 'mvn test'
                }
            }
        }
        stage('Docker Build') {
            steps {
                script {
                    sh 'docker build -t $IMAGE .'
                }
            }
        }
        stage('Docker Run') {
            steps {
                script {
                    sh 'docker rm -f mycontainer || true'
                    sh 'docker run -d --name demo -p 8080:9999 demo-app'
                }
            }
        }
        stage('Dockerhub login') {
            steps {
                script {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                }
            }
        }
        stage('Push Dockerhub') {
            steps {
                script {
                sh 'docker tag springboot-app veera03007/springboot-app:latest'
                sh 'docker push veera03007/springboot-app:latest'
                }
            }
        }
        post {
        always {
            sh 'docker logout'
        }
        }
         stage('Deploy') {
            steps {
                script {
                     sh 'cp $BUILD_JAR $APP_DIR/$JAR_NAME'
                }
            }
        }
    }
















