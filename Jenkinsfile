pipeline {
    agent any

    stages {
        stage('Compile') {
            steps {
                sh './mvnw clean compile -e'
            }
        }
        stage('Unit Test') {
            steps {
                sh './mvnw clean test -e'
            }
        }
        stage('Package') {
            steps {
                sh './mvnw clean package -e'
            }
        }

        stage('Sonarqube') {
            steps {
                withSonarQubeEnv('Sonar') { // You can override the credential to be used
                    sh './mvnw org.sonarsource.scanner.maven:sonar-maven-plugin:3.7.0.1746:sonar'
                }
            }
        }

        stage('Run') {
            steps {
                sh 'nohup bash mvnw spring-boot:run &'
            }
        }
        stage('Test') {
            steps {
                script{
                    sleep 60
                    final String url = 'http://localhost:8081/rest/mscovid/test?msg=testing'
                    final String response = sh(script: "curl -X GET $url", returnStdout: true).trim()

                    echo response
                }
            }
        }
    }
}
