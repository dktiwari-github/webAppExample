pipeline {
    agent any

    tools {
        maven "maven_3_0_5"
    }

    stages {
        stage('Build') {
            steps {

                sh "mvn -Dmaven.test.failure.ignore=true clean package"
            }
        

            post {
                success {
                    junit allowEmptyResults: true, testResults: '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.war'
                }
            }
        }
        
        
    
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            
            post {
                always {
                    junit allowEmptyResults: true, testResults: 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deploy') {
            steps {
                deploy adapters: [tomcat8(credentialsId: 'd22f3f0a-5b22-4e45-a1ea-525f776e344d', path: '', url: 'http://192.168.16.164:9090')], contextPath: 'webAppExample', war: '**/*.war'
            }
            post {
                success{
                    emailext body: 'Deployment is successful', recipientProviders: [developers()], subject: 'webAppExample', to: 'dheeraj.tiwari@afourtech.com'
                }
            }
        }
    }
}
