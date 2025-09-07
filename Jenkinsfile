pipeline {
  agent any

  environment {
    MAVEN_OPTS = '-Xmx1024m'
    SONAR_URL   = 'http://51.20.67.145:9000'  // Pro 2 SonarQube
    WAR_GLOB    = 'target/*.war'
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build & Test') {
      steps {
        sh 'mvn -B -DskipTests=false clean package'
        junit '**/target/surefire-reports/*.xml'
      }
    }

    stage('SonarQube Analysis') {
      steps {
        withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
          sh "mvn -B sonar:sonar -Dsonar.host.url=${SONAR_URL} -Dsonar.login=${SONAR_TOKEN}"
        }
      }
    }

    stage('Deploy to Tomcat') {
      steps {
        script {
          def war = sh(script: "ls ${WAR_GLOB} | head -n 1", returnStdout: true).trim()
          if (!war) { error "WAR not found" }
          sh "cp ${war} /opt/tomcat/webapps/"
        }
      }
    }

    stage('Smoke Test') {
      steps {
        sh 'curl -sS --fail http://localhost:8080/ || true'
      }
    }
  }
}
