pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/ibrawidadi/ChakraHs-Laravel_CI-CD_Sonar.git', branch: 'main'
            }
        }

        stage('Install Deps') {
            steps {
                echo 'composer install'
            }
        }
        
        stage('Build Laravel') {
            steps {
                echo 'php artisan serve &'
            } 
        } 

        stage('Install Node Dependencies') {
            steps {
                echo 'npm install'
            }
        }

        stage('Compile Node Assets') {
            steps { 
                echo 'npm run build'
            } 
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'sonar-scanner'
                    withSonarQubeEnv('SonarQube') {
                        sh """
                            ${scannerHome}/bin/sonar-scanner \
                            -Dsonar.projectKey=qualitytest1 \
                            -Dsonar.host.url=http://localhost:9000 \
                            -Dsonar.login=sqa_0080af81d73aac8be61137fed7fca0ad3cda2f3e \
                            -Dsonar.sources=./app \
                            -Dsonar.exclusions="vendor/,storage/,bootstrap/cache/"
                        """
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }

    post {
        always {
            echo "Analysis complete. Check SonarQube for results."
        }
    }
}

