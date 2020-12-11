pipeline {
    agent any

    stages {
        stage('Compile') {
            steps {
                sh './mvnw clean compile -e'
            }
        }
        stage('Test') {
            steps {
                sh './mvnw clean test -e'
            }
        }
        stage('Jar') {
            steps {
                sh './mvnw clean package -e'
            }
        }

        stage('Sonar') {
            steps {
                withSonarQubeEnv('Sonar') { // You can override the credential to be used
                    sh './mvnw org.sonarsource.scanner.maven:sonar-maven-plugin:3.7.0.1746:sonar'
                }
            }
        }

        stage('UploadNexus') {
            steps {
                nexusPublisher nexusInstanceId: 'Nexus_server_ubuntu', nexusRepositoryId: 'test-repo', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: 'jar', filePath: '/Users/dmorales/Documents/diplomadodevops2020/ejemplo-maven/build/DevOpsUsach2020-0.0.1.jar']], mavenCoordinate: [artifactId: 'DevOpsUsach2020', groupId: 'com.devopsusach2020', packaging: 'jar', version: '0.0.1']]], tagName: '0.0.1'
            }
        }

    }
}
