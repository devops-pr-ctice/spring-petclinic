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
        stage('Build'){
            steps{
                sh 'mvn clean package'
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