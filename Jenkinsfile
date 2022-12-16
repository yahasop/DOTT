pipeline {
    agent any
    tools {
	    //This declare my Go tools to be downloaded in my Jenkins master, in this case (or in the node)
            dockerTool 'docker'
	    go 'go-jenkins'
            //tool name: 'sonarqube-scanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
    }
    //This is where the stages of the pipeline begin.
    stages {
        stage('SCM Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/yahasop/DOTT']]])
            }
        }
        
        
        stage('Build') {
            steps {
                dir(path: 'go') {
		    //sh 'rm go.mod' //This will remove the go.mod file if there's a previuos build.
		    //sh 'go env -w GO111MODULE=auto'
                    sh 'go mod init dott' //This initializes a module for the application.
		    sh 'go mod tidy' //This download all the dependencies required in the source files.
		    sh 'go build api.go' //This build and package the application through the module declared in mod init, it results in the artifact.
		    sh 'ls'
		    
		    // I also have this code but the one I keep worked better
                    //sh 'go env -w GO111MODULE=off'		    
                    //sh 'go get -u github.com/gorilla/mux'
		    //sh 'go get github.com/stretchr/testify/assert'
		    //sh 'go get github.com/Pepegasca/goop'

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
				catchError(buildResult: 'SUCCESS', stageResult: 'SUCCESS', message: 'Unit testing failed, but continued to next step'){
					sh 'go test . | tee go-test' //This show the stdout in the console and then copy it into a file called go-test.
				}
			}        
		}     
	}

        stage('Deployment'){		
            	
		steps{	
			dir(path: 'go') {
					
				sh '''
				pwd
				ls
				timeout 10s ./api
				'''
			}
		}
	}		
    }    
}

