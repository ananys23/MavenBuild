pipeline {
    agent any

    stages {
        stage('Code Checkout') {
            steps {
                checkout changelog: false, poll: false, scm: scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'githubcred', url: 'https://github.com/ananys23/MavenBuild']])
            }
        }

        stage('Build Automation') {
            steps {
                script {
                    def mvnHome = tool 'Maven'
                    sh "${mvnHome}/bin/mvn clean install"
                }
            }
        }

        stage('Code Deployment') {
            steps {
                script {
                    deploy adapters: [tomcat9(credentialsId: 'TomcatCreds', path: '', url: 'http://13.48.249.10:8080/')], contextPath: 'Planview', onFailure: false, war: 'target/*.war'
                }
            }
        }
    }
}
