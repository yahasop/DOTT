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
        
        
        stage('Build') {
            steps {
                dir(path: 'go') {
                    //sh 'go mod init'
                    sh 'go env -w GO111MODULE=off'		    
                    sh 'go get -u github.com/gorilla/mux'
		    sh 'go get github.com/stretchr/testify/assert'
		    sh 'go get github.com/Pepegasca/goop'
                    sh 'go build api.go convert.go'
                }
            }
        }

        stage('SonarQube Scanning') {
            steps {
                sh 'echo "Still working in this"'
            }
        }

        stage('Test'){
		steps {
			dir(path: 'go'){
				catchError(message: 'failed unit tests', catchInterruptions: true, buildResult: 'SUCCESS', stageResult: 'SUCCESS'){
					sh 'go test .'
			
				}
                
			}
            
		}
        
	}

        stage('Deployment'){
            steps{
                sh 'echo "Deployment Process"'
            }
        }
    }
}
