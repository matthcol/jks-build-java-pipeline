def remote = [:]
remote.name = 'test'
remote.host = '192.168.1.63'
remote.port = 22
// remote.user = 'moi'
// remote.password = 'password'
remote.allowAnyHosts = true
remote.timoutSec = 3000



pipeline {

    agent any
	
		parameters {
			string(name: 'REPO', defaultValue: 'https://github.com/matthcol', description: '')	
			string(name: 'PROJECT', defaultValue: 'mc2020.cinema.git', description: '')	
			string(name: 'BRANCH', defaultValue: 'qualif', description: '')
			string(name: 'PACKAGE', defaultValue: 'cinema.jar', description: '')
		}

		tools {
			maven "M3"
		}

		stages {
			stage('clone') {
				steps {
					git url: "${params.REPO}/${params.PROJECT}",
						branch: "${params.BRANCH}"
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
		
		stage('publish') {
			steps {
				script {
					withCredentials([sshUserPrivateKey(credentialsId: 'sshTest', keyFileVariable: 'identity', passphraseVariable: 'passphrase', usernameVariable: 'userName')]) {
						remote.user = userName
						remote.identityFile = identity
						remote.passphrase = passphrase

						echo 'deploy artifact'
						sshPut remote: remote, from: "target/${params.PACKAGE}", into: 'repo-artifact-mc'
					}
				}
			}
		}
	}
}