pipeline {
    agent any

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
    }
}