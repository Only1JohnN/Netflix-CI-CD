pipeline {
    agent any
    
    environment {
        // Set the GitHub repository URL
        GIT_URL = 'https://github.com/Only1JohnN/Netflix-CI-CD.git'
        PATH = "/usr/local/apache-maven-3.9.7/bin:$PATH"
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

        stage('Merge Check') {
            steps {
                // Check if the pull request can be merged
                script {
                    def pullRequestStatus = sh(script: 'git status', returnStatus: true)
                    if (pullRequestStatus == 0) {
                        echo 'Pull request can be merged'
                    } else {
                        echo 'Pull request cannot be merged'
                        // Perform actions like notifying developers or rejecting the pull request
                    }
                }
            }
        }
        
        stage('Software Versions & Build Maven Project') {
            steps {
                echo 'Hello World from Jenkins Pipeline'
                sh 'java -version'
                sh 'git --version'
                sh 'mvn --version'
                sh 'mvn clean install package'
            }
        }
        
        stage('Build Verisons') {
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
