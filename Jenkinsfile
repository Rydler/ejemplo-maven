pipeline {
    agent any

    stages {
        stage('Compile') {
            steps {
                dir('/Users/dmorales/Documents/diplomadodevops2020/ejemplo-maven'){
                    sh './mvnw clean compile -e'
                }
            }
        }
        stage('Unit Test') {
            steps {
                dir('/Users/dmorales/Documents/diplomadodevops2020/ejemplo-maven'){
                    sh './mvnw clean test -e'
                }
            }
        }
        stage('Package') {
            steps {
                dir('/Users/dmorales/Documents/diplomadodevops2020/ejemplo-maven'){
                    sh './mvnw clean package -e'
                }
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
                dir('/Users/dmorales/Documents/diplomadodevops2020/ejemplo-maven'){
                    sh 'nohup bash mvnw spring-boot:run &'
                }
            }
        }
        stage('Test') {
            steps {
                script{
                    dir('/Users/dmorales/Documents/diplomadodevops2020/ejemplo-maven'){
                        sleep 60
                        final String url = 'http://localhost:8081/rest/mscovid/test?msg=testing'
                        final String response = sh(script: "curl -X GET $url", returnStdout: true).trim()

                        echo response
                    }
                }
            }
        }
    }
}
