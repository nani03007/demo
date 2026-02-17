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
        DOCKER_CREDS = credentials('dockerhub')
        DOCKER_CREDS_PSW="Naruto@7019"
        DOCKER_CREDS_USR="veera03007"
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
                sh "mvn clean install -DskipTests=true"
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
                    sh 'docker run -d --name demo -p 8080:9999 $IMAGE'
                }
            }
        }
        stage('Dockerhub login') {
            steps {
                script {
                    sh 'echo $DOCKER_CREDS_PSW | docker login -u $DOCKER_CREDS_USR --password-stdin'
                }
            }
        }
        stage('Push Dockerhub') {
            steps {
                script {
                    sh 'docker push $IMAGE'
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












