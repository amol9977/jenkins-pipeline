pipeline {

    // The agent name must match with the jenkins node name (Manage jenkins -> Nodes)
    agent {
        node {
            label 'maven-build-server'
        }
    }

    // The tool name must match with the jenkins tools (global configuration) variable names
    tools { 
        maven 'maven-3.9.8' 
    }

    options {
        buildDiscarder logRotator( 
                    daysToKeepStr: '15', 
                    numToKeepStr: '10'
            )
    }
    
    // Define environment variables
    environment {
        APP_NAME = "BINDESH_APP"
        APP_ENV  = "PRODUCTION"
    }

    // Cleanup the jenkins workspace before building an Application
    stages {
        
        stage('Cleanup Workspace') {
            steps {
                cleanWs()
                sh """
                echo "Cleaned Up Workspace for ${APP_NAME}"
                """
            }
        }

        // Invoke app source code from the Github repository
        stage('Code Checkout') {
            steps {
                checkout([
                    $class: 'GitSCM', 
                    branches: [[name: '*/main']], 
                    userRemoteConfigs: [[url: 'https://github.com/kbindesh/mvn-lab-project.git']]
                ])
            }
        }

        // Build the application code using Maven
        stage('Code Build') {
            steps {
                 sh 'mvn install -Dmaven.test.skip=true'
            }
        }
 
        // This stage is added only to demostrate parallel stage execution
        stage('Environment Analysis') {

            parallel {

                stage('Priting All Global Variables') {
                    steps {
                        sh """
                        env
                        """
                    }
                }

                stage('Execute Shell') {
                    steps {
                        sh 'echo "Hello"'
                    }
                }

                stage('Print Env Variables') {
                    steps {
                        sh "echo ${APP_ENV}"
                    }
                }

            
            }
        }

    }   
}
