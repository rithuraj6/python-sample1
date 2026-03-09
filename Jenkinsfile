pipeline {
    agent any
    
    stages {
        stage("Checkout Code") {
            steps {
                checkout scm
            }

        }

        stage ("Install Dependencies"){
            steps {
                sh '''
                . venv/bin/activate
                coverage run -m pytest
                coverage  xml
                '''
         
            }

        }
        stage("SonarQube Analysis"){
            steps{
                withSonarQubeEvn('SonarQube'){
                    sh '''
                    . venv/bin/activate
                    sonar-scanner
                    '''
                }
            }
        }
    }
}