pipeline {
    agent { label 'Slave'}
    tools {
        jdk 'Java17'
        maven 'maven3'
    }
    
    stages{
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/ridodomayana/register-app.git'
            }
        }

        stage("Build Application") {
            steps {
                sh "mvn clean package"
            }
        }

        stage("Test Application") {
            steps {
                sh "mvn test"
            }
        }

        stage("SonarQube Analysis") {
            steps {
                script{
                    withSonarQubeEnv(credentialsId: 'Jenkins-Sonar') {
                        sh "mvn sonar:sonar"
                    }
                }
            }
        }

        stage("Quality Gate") {
            steps {
                script{
                    waitForQualityGate abortPipeline: false, credentialsId: 'Jenkins-Sonar'
                }
            }
        }


    }
}
