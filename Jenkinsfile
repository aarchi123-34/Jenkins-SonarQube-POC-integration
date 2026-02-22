pipeline {
    agent any

    tools {
        sonarQubeScanner 'sonar-scanner'
    }

    environment {
        SONAR_AUTH_TOKEN = credentials('sonarcloud-token')
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building project...'
                sh 'python3 -m pip install --user -r requirements.txt || true'
            }
        }

        stage('SonarCloud Analysis') {
            steps {
                withSonarQubeEnv('sonarcloud') {
                    sh '''
                    sonar-scanner \
                      -Dsonar.projectKey=aarchi123-34_Jenkins-SonarQube-POC-integration \
                      -Dsonar.organization=aarchi123-34 \
                      -Dsonar.sources=. \
                      -Dsonar.host.url=https://sonarcloud.io \
                      -Dsonar.login=$SONAR_AUTH_TOKEN
                    '''
                }
            }
        }

        stage('Quality Gate') {
            steps {
                waitForQualityGate abortPipeline: true
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploy stage'
            }
        }
    }
}
