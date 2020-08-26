pipeline {
    agent any

    stages {
        stage('compile') {
            steps {
                echo 'compile'
                bat 'mvn compile'
            }
        }
        stage('package') {
            steps {
                echo 'package'
            }
        }
    }
}