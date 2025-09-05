pipeline {
    agent any

    tools {
        maven 'Maven3'
        jdk 'JDK17'
    }

    environment {
        DEPLOY_DIR = "/opt/tomcat10/webapps"
        WAR_FILE   = "target/NumberGuessGame-1.0-SNAPSHOT.war"
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
                        $DEPLOY_DIR/../bin/shutdown.sh || true
                        $DEPLOY_DIR/../bin/startup.sh
                    '''
                }
            }
        }
    }

    post {
        success {
            echo "✅ Build and deployment successful!"
        }
        failure {
            echo "❌ Build or deployment failed. Check logs."
        }
    }
}
