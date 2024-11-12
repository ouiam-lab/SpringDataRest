pipeline {
    agent any

    tools {
        maven 'maven3' // Ensure the name is correct
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/oumaima33/SpringDataRest.git', branch: 'master'
            }
        }

        stage('Compile') {
            steps {
                script {
                    def mvnHome = tool 'Maven_Local'
                    bat "\"${mvnHome}\\bin\\mvn\" clean compile"
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'sonar-scanner'
                    withSonarQubeEnv('SonarQube') { // Ensure 'SonarQube' matches Jenkins config
                        bat """
                            "${scannerHome}\\bin\\sonar-scanner.bat" ^
                            -Dsonar.projectKey=ouiam ^
                            -Dsonar.host.url=http://localhost:9000 ^
                            -Dsonar.login=sqp_7b723ce2f82ef1574adab29a3dd460253ad4cb82 ^
                            -Dsonar.sources=./src ^
                            -Dsonar.java.binaries=./target/classes
                        """
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 10, unit: 'MINUTES') { // Increase if needed
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
