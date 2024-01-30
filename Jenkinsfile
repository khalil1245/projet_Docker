pipeline {
  agent any
  tools {
    maven "M3"
  }

  environment {
      def mvn = tool 'M3'

      NEXUS_VERSION = "nexus3"
      NEXUS_PROTOCOL = "http"
      NEXUS_URL = "172.17.0.4:8081"
      NEXUS_REPOSITORY = "repoJenkinsLy2"
      NEXUS_CREDENTIAL_ID = "nexusCredential"
      ARTIFACT_VERSION = "${BUILD_NUMBER}"
  }
  
  stages {
    stage('Git Check out') {
      steps{
        checkout scm
      } 
    }

    stage('Maven build') {
      steps {
        sh "${mvn}/bin/mvn clean package "
      }
    }

    stage('SonarQube Analysis') {
      steps{
        withSonarQubeEnv('sonar-server') {
        sh "${mvn}/bin/mvn clean verify sonar:sonar -Dsonar.projectKey=MonprojetDocker -Dsonar.projectName='MonprojetDocker'"
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
                      nexusUrl: '172.17.0.3:8081', 
                      nexusVersion: 'nexus3', 
                      protocol: 'http', 
                      repository: 'app01',
                      version: '0.1'
                     
              )
        }
 }
}
