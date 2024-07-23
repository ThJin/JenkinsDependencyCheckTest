pipeline {
	agent any
	stages {
		stage('Checkout SCM') {
			steps {
				git url: 'https://github.com/ThJin/JenkinsDependencyCheckTest.git'

			}
		}
		
        stage('Update NVD Data') {
            steps {
                script {
                    docker.image('owasp-dependency-check:10.0.3').inside('--volume dependency-check-cache:/dependency-check/data') {
                        sh 'dependency-check.sh --updateonly --data /dependency-check/data'
                    }
                }
            }
        }		

		stage('OWASP Dependency-Check Vulnerabilities') {
			steps {
				dependencyCheck additionalArguments: '''
							-o './'
							-s './'
							-f 'ALL'
							--nvdApiDelay 8000
							--prettyPrint''', odcInstallation: 'OWASP Dependency-Check Vulnerabilities'
				
				dependencyCheckPublisher pattern: 'dependency-check-report.xml'
			}
		}
	}	
	post {
		success {
			dependencyCheckPublisher pattern: 'dependency-check-report.xml'
		}
	}
}