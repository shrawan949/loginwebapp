pipeline {
    agent any

    tools{
        jdk 'java-17'
        maven 'Maven3.9'
    }

    environment {
        SCANNER_HOME = tool 'sonar-scanner'
    }

    stages{

        stage("Git Checkout"){
            steps{
               git 'https://github.com/shrawan949/loginwebapp.git'
            }
        }

        stage("Compile"){
            steps{
                sh "mvn clean compile"
            }
        }

        stage("Test Cases"){
            steps{
                sh "mvn test"
            }
        }

        stage("Sonarqube Analysis"){
            steps{
                withSonarQubeEnv('sonar-server') {
                    sh '''
                    $SCANNER_HOME/bin/sonar-scanner \
                    -Dsonar.projectName=Loginwebapp \
                    -Dsonar.java.binaries=. \
                    -Dsonar.projectKey=Loginwebapp
                    '''
                }
            }
        }

        stage("Quality Gate"){
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'Sonar-token'
                }
            }
        }

        stage("Build"){
            steps{
                sh "mvn clean install"
            }
        }
    }
}
