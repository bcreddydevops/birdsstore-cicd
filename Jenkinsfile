pipeline {
    agent any
    tools {
        jdk 'jdk17'
        maven 'maven3'
    }

    stages {
        stage('Clean WorkSpace') {
            steps {
                cleanWs()
            }
        }
        stage('Checkout SourCode') {
            steps {
                git 'https://github.com/bcreddydevops/birdsstore-cicd.git'
            }
        }
        stage('Maven Compile') {
            steps {
                sh 'mvn clean compile'
            }
        }
        stage('Maven Test') {
            steps {
                sh 'mvn test'
            }
        }
       stage("Sonarqube analysis"){
            steps{
                script{
                withSonarQubeEnv(installationName: 'sonar-server', credentialsId: 'Sonar-token') {
                      sh 'mvn sonar:sonar'
                  }

                   timeout(5) {
                      def qg = waitForQualityGate()
                      if (qg.status != 'OK') {
                           error "Pipeline aborted due to quality gate failure: ${qg.status}"
                      }
                    }
                }
            }
        }
    }

}
