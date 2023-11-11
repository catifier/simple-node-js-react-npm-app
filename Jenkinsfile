pipeline {
    agent none
    stages {
        stage('Build') {
		agent {
        docker {
            image 'node:20.9.0-alpine3.18'
            args '-p 3000:3000'
        }
    }
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
		agent {
        docker {
            image 'node:20.9.0-alpine3.18'
            args '-p 3000:3000'
        }
    }
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Deliver') {
		agent {
        docker {
            image 'node:20.9.0-alpine3.18'
            args '-p 3000:3000'
        }
    }
            steps {
                sh './jenkins/scripts/deliver.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
	stage('OWASP-DC') {
		agent any
	    steps {
		dependencyCheck additionalArguments: ''' 
			    --disableYarnAudit
		            -o './'
		            -s './'
		            -f 'ALL' 
		            --prettyPrint''', odcInstallation: 'OWASP-DC'
		
		dependencyCheckPublisher pattern: 'dependency-check-report.xml'
	    }
	}
    }
}
