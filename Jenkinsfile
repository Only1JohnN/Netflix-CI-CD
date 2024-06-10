pipeline {
    agent any

    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }
        
        stage('Checkout from Git') {
            steps {
                git branch: 'main', url: 'https://github.com/Only1JohnN/Netflix-CI-CD.git'
            }
        }
        
        stage('Hello') {
            steps {
                echo 'Hello World from Jenkins Pipeline'
                //echo java -version
                //echo 'git --version'
            }
        }
        
        stage('Software Versions') {
            steps {
                echo env.BUILD_ID
                echo env.BUILD_NUMBER
                echo env.JENKINS_HOME
            }
        }
    }
}
