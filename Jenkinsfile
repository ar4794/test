pipeline {
    agent any

    environment {
        GIT_REPO_URL='https://github.com/ar4794/test'
    }
    
    stages {
        stage('checkout') {
            steps {
                     script {
                         checkout ($class: 'GitSCM', branches: [[name: '/dev' ]], doGenerateSubmoduleConfiguartion: false, extensions:[], submoduleCfg: [],  userRemoteConfigs: [[url:  GIT_REPO_URL]])
                     }
                }
            }
        }
        stage('List folders') {
            steps {
                script {
                    def folder= sh (script: 'ls -d */', returnStdout: true).trim()
                    echo "Folders in the reporsitory: ${folder}"
                }
            }
        }

    }

