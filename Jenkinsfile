pipeline {
    agent any
    tools {
        maven 'M3'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/khalil1245/projet_Docker.git/'
            }
        }
        stage('Build Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                 withSonarQubeEnv(installationName: 'sonar-server', credentialsId: 'khalill') {
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage("publish to nexus") {
            steps {
                script {
                    nexusArtifactUploader(
                        artifacts: [[
                            artifactId: 'toto-gros', 
                            classifier: '', 
                            file: 'target/toto-gros-0.1.jar',
                            type: 'jar'
                        ]], 
                        credentialsId: 'credentials_nexus',
                        groupId: 'org.groupeisi', 
                        nexusUrl: '172.17.0.2:8081', 
                        nexusVersion: 'nexus3', 
                        protocol: 'http', 
                        repository: 'app01',
                        version: '0.1'
                    )
                }
            }
        }
    }
}
