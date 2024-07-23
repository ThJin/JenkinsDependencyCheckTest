pipeline {
	agent any
	stages {
		stage('Checkout SCM') {
			steps {
				git url: 'https://github.com/ThJin/JenkinsDependencyCheckTest.git'

			}
		}
		
        stage('Purge Dependency-Check Cache') {
            steps {
                script {
                    sh 'dependency-check.sh --purge'

                }
            }
        }

		stage('OWASP Dependency-Check Vulnerabilities') {
			steps {
				dependencyCheck additionalArguments: '''
							-o './'
							-s './'
							-f 'ALL'
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