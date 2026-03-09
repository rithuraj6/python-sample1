pipeline {
    agent any
    
    stages {

        stage("Checkout Code") {
            steps {
                checkout scm
            }
        }

        stage("Install Dependencies") {
            steps {
                sh '''
                python3 -m venv venv
                . venv/bin/activate
                pip install -r requirements.txt
                '''
            }
        }

        stage("Run Tests") {
            steps {
                sh '''
                . venv/bin/activate
                coverage run -m pytest
                coverage xml
                '''
            }
        }

        stage("SonarQube Analysis") {
            steps {
                script {
                    def scannerHome = tool 'sonar-scanner'
                    withSonarQubeEnv('SonarQube') {
                        sh """
                        . venv/bin/activate
                        ${scannerHome}/bin/sonar-scanner
                        """
                    }
                }
            }
        }
        stage("Quality Gate"){
            steps {
                timeout(time:5, unit: "MINUTES"){
                    waitForQualityGate abortPipeline: true
                }
            }
        }      

    }
}
