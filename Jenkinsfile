pipeline {
    agent any
    tools {
        go 'go-jenkins'
        //tool name: 'sonarqube-scanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
    }
    stages {
        stage('SCM Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/yahasop/DOTT']]])
            }
        }
        
        stage('SonarQube Scanning') {
            steps {
                def scannerHome = tool 'sonarqube-scanner';
                withSonarQubeEnv('sonarcloud') { // If you have configured more than one global server connection, you can specify its name
                    sh "${scannerHome}/bin/sonar-scanner"
                    sh 'echo "Still working in this"'
            }
        }
        
        stage('Build') {
            steps {
                dir(path: 'go') {
                    //sh 'go mod init'
                    sh 'go env -w GO111MODULE=off'
                    sh 'go get -u github.com/gorilla/mux'
                    sh 'go build api.go convert.go'
                }
            }
        }
    }
}
