pipeline {
    agent any

    tools {
        maven 'Maven-3.9.11'   // the name you configured in Jenkins
        jdk 'Java-17'          // the name you configured in Jenkins
    }

    environment {
        DEPLOY_DIR = "/opt/tomcat10/webapps"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/OmastolaT/NumberGuessGame.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                script {
                   sh '''
                    rm -rf $DEPLOY_DIR/NumberGuessGame*
                    cp $WAR_FILE $DEPLOY_DIR/
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Build and deployment successful!'
        }
        failure {
            echo '❌ Build or deployment failed. Check logs.'
        }
    }
}
