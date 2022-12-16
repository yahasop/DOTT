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
		    sh 'rm go.mod' //This will remove the go.mod file if there's a previuos build.
		    sh 'go env -w GO111MODULE=auto'
                    sh 'go mod init dott' //This initializes a module for the application.
		    sh 'go mod tidy' //This download all the dependencies required in the source files.
		    sh 'go build' //This build and package the application through the module declared in mod init, it results in the artifact.
		    
		    // I also have this code but the one I keep worked better
                    //sh 'go env -w GO111MODULE=off'		    
                    //sh 'go get -u github.com/gorilla/mux'
		    //sh 'go get github.com/stretchr/testify/assert'
		    //sh 'go get github.com/Pepegasca/goop'
                    //sh 'go build api.go convert.go'
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
		agent {
			dockerfile true
		}	
            
		steps{
		    /*sh 'sudo chown $USER /var/run/docker.sock'
		    sh 'chmod 777 /var/run/docker.sock' //I know this is not recommended at all but I want to run docker without provisioning credentials or adding jenkins to sudoers or adding docker group.
		    sh 'sudo gpasswd -a jenkins root'
		    sh 'sudo service docker restart'
		    sh 'sudo usermod -aG docker jenkins'
		    sh 'reboot'*/
		    
		    sh 'docker build go/. -t goapp'
		    sh 'docker images'
		    
		    /*withDockerRegistry(credentialsId: 'dockerhubcreds', usernameVariable: 'dockerHubPassword', passwordVariable: 'dockerHubUser' url: 'https://hub.docker.com/') {
			    sh "docker login -u${env.dockerHubUser} -p ${env.dockerHubPassword}"
			    sh 'docker push goapp'
		    } 
		    
		    withCredentials([usernamePassword(credentialsId: 'dockerhubcreds', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
			    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
			    sh 'docker push goapp'
		    }  */
	    }
	}		
    }    
}

