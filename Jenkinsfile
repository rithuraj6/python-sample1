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
                . djvenv/bin/activate
                coverage run -m pytest
                coverage  xml
                '''
         
            }

        }
        stage("SonarQube Analysis"){
            steps{
                withSonarQubeEvn('SonarQube'){
                    sh '''
                    . djvenv/bin/activate
                    sonar-scanner
                    '''
                }
            }
        }
    }
}
