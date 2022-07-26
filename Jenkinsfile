// Declarative pipeline
pipeline {
    agent any
    stages {
      	
	stage ('Build') {
	   steps {
	      sh 'pyb init clean analyze verify coverage'
           }
       	}
	stage('SonarQube Analysis') {
	    environment {
		scannerHome = tool 'SonarQube Scanner'
	    }
	    steps {				
		withSonarQubeEnv('admin') {
			sh '${scannerHome}/bin/sonar-scanner \
			-D sonar.projectKey=pybuilder \
			-D sonar.python.coverage.reportPaths=/target/reports/*.xml'
		}
            }
	}
        stage ('Generate Test Reports') {
            steps {
                sh 'pyb publish'       
            }
        }
	stage ('Publish Artifactory') {
	    steps {
		withCredentials([usernamePassword(credentialsId: 'artifactory', passwordVariable: 'passwd', usernameVariable: 'user')]) {
			sh 'jf rt upload target/dist/sampledevopsproject-1.0/dist/sampledevopsproject-1.0.tar.gz pythonapplication/'
		}	
	    }
	}
    }
}
