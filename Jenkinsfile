def remote = [:]
remote.name = 'test'
remote.host = '192.168.1.73'
remote.port = 22
// remote.user = 'moi'
// remote.password = 'password'
remote.allowAnyHosts = true
remote.timoutSec = 3000



pipeline {

    agent { node {
		label: 'with-ssh'
		withCredentials([sshUserPrivateKey(credentialsId: 'sshTest', keyFileVariable: 'identity', passphraseVariable: 'passphrase', usernameVariable: 'userName')]) {
			remote.user = userName
			remote.identityFile = identity
			remote.passphrase = passphrase
			}
		}
	}
	

		tools {
			maven "M3"
		}

		stages {
			stage('clone') {
				steps {
					git url: 'https://github.com/matthcol/jks-geometry.git' 
				}
			}
			stage('compile') {
				steps {
					echo 'compile'
					bat 'mvn clean compile'
				}
			}
			stage('test') {
				steps {
					echo 'test'
					bat 'mvn -Dmaven.compile.skip=true test'
					// echo 'step after test in stage test'
				}
				post {
					always {
						echo 'always after test'
						//JUnitResultArchiver testResults: 'target/surefire-reports/TEST*.xml'
						junit 'target/surefire-reports/TEST*.xml'
					}	
					success {
						echo 'test stage successfull'
					}
					failure {
						echo 'test stage failure'
					}
					regression {
						echo 'test stage regression'
					}
				}
			}
			stage('package') {
				steps {
					echo 'package'
					bat 'mvn -Dmaven.test.skip=true package'
				}
			}
		
		stage('deploy') {
			steps {
				echo 'deploy artifact'
				sshCommand remote: remote, command: "ls -lrt"
			}
		}
	}
}