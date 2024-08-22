pipeline{
    agent{
        label 'devnode'
    }
    tools{
        maven 'MAVEN_3.8.8'
        jdk 'jdk-17'
    }
    stages{
        stage('SCM'){
            steps{
                git url: 'https://github.com/devops-pr-ctice/spring-petclinic.git',
                branch: 'main'
            }
        }
        stage('SonarQube analysis'){
            steps{
                withSonarQubeEnv(credentialsId: '7a594c121196669e3815a15fa63a96c83b49d992', installationName: 'SONARCUBE') { // You can override the credential to be used
                sh 'mvn clean package org.sonarsource.scanner.maven:sonar-maven-plugin:3.7.0.1746:sonar'
                }
                junit testResults: '**/surefire-reports/*.xml'
                archiveArtifacts artifacts: '**/target/spring-petclinic-*.jar'
            }
        }
        stage("Quality Gate") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
                    // true = set pipeline to UNSTABLE, false = don't
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
    post{
        success{
            junit testResults: '**/surefire-reports/*.xml'
            archive '**/target/spring-petclinic-*.jar'
        }
    }
}