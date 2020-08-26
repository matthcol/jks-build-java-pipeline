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
				echo 'step after test in stage test'
			}
		}
        stage('package') {
            steps {
                echo '''package
				2 ligne
				3e ligne
				3e ligne'''
            }
        }
    }
}