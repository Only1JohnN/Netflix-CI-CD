pipeline {
    agent any
    
    environment {
        // Set the GitHub repository URL
        GIT_URL = 'https://github.com/Only1JohnN/Netflix-CI-CD.git'
    }

    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }
        
        stage('Checkout from Git') {
            steps {
                script {
                    if (env.CHANGE_ID) {
                        // This is a pull request
                        echo "This is a pull request: ${env.CHANGE_ID}"
                        echo "Source branch: ${env.CHANGE_BRANCH}"
                        echo "Target branch: ${env.CHANGE_TARGET}"
                        checkout([
                            $class: 'GitSCM', 
                            branches: [[name: "origin/${env.CHANGE_BRANCH}"]],
                            doGenerateSubmoduleConfigurations: false,
                            extensions: [[$class: 'CleanCheckout']],
                            submoduleCfg: [],
                            userRemoteConfigs: [[url: env.GIT_URL]]
                        ])
                    } else {
                        // This is not a pull request
                        echo "This is not a pull request"
                        checkout([
                            $class: 'GitSCM', 
                            branches: [[name: "origin/main"]],
                            doGenerateSubmoduleConfigurations: false,
                            extensions: [[$class: 'CleanCheckout']],
                            submoduleCfg: [],
                            userRemoteConfigs: [[url: env.GIT_URL]]
                        ])
                    }
                }
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
                echo "BUILD_ID: ${env.BUILD_ID}"
                echo "BUILD_NUMBER: ${env.BUILD_NUMBER}"
                echo "JENKINS_HOME: ${env.JENKINS_HOME}"
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline succeeded!'
            // Add any success notifications or actions here
        }
        failure {
            echo 'Pipeline failed!'
            // Add any failure notifications or actions here
        }
    }
}
