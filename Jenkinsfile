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

        stage('compile') {
            steps {
                echo 'compile'
                bat 'mvn compile'
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