// Declarative pipeline
pipeline {
    agent any
    stages {
       
		stage ('Build & Test') {
	        steps {
				echo '********* Test Stage Started **********'
				sh 'pyb clean analyze verify'
				echo '********* Test Stage Finished **********'
			}
		}
		
		stage('SonarQube Analysis') {
			environment {
				scannerHome = tool 'SonarQube Scanner'
    			}
			steps {
				echo '********* Test Stage Started **********'
				
				withSonarQubeEnv('admin') {
					sh '${scannerHome}/bin/sonar-scanner \
					-D sonar.projectKey=pybuilder'
				}
				echo '********* Test Stage Finished **********'
			}
		}
        stage ('Generate Test Reports') {
            steps {
                sh 'pyb publish'       
            }
        }
		stage ('Publish Artifactory') {
		    steps {
				echo '********* Publish Report to JFrog Artifacts **********' 
				withCredentials([usernamePassword(credentialsId: 'artifactory', passwordVariable: 'passwd', usernameVariable: 'user')]) {
					sh 'jf rt upload target/reports/ pythonapp/'
				}
				echo '********* Publish Report Finished **********'	
			}
		}
	}
}
