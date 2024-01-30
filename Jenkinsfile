pipeline {
    agent any

    stages {
        stage('Get Git Tag') {
            steps {
                script {
                env.TAG_NAME = sh(returnStdout: true, script: 'git tag --points-at HEAD | awk \'NR == 1 {print}\'').trim()
                echo "env.TAG_NAME=${env.TAG_NAME}"
                } // script
            } // steps
        }
        stage("print"){
            steps{
                sh "git branch"
                echo "${env.TAG_NAME}"
            }
        }
        stage('Check if tag is pushed') {
            when {
                expression { 
                    return env.BRANCH_NAME == 'develop' && env.TAG_NAME != ''
                }
            }

            steps {
                echo "Tag pushed on develop branch: ${env.TAG_NAME}"
                // Add your build steps here
            }
        }

        stage('Clone Code') {
            when {
                tag "*-rc"
            }

            steps {
                script {
                    // Clone the code from GitHub using the provided credentials
                    checkout([$class: 'GitSCM', 
                              branches: [[name: 'develop']], 
                              doGenerateSubmoduleConfigurations: false, 
                              extensions: [[$class: 'CloneOption', depth: 1, noTags: false, shallow: true, reference: '', timeout: 120]], 
                              submoduleCfg: [], 
                              userRemoteConfigs: [[credentialsId: 'token', url: 'https://github.com/ghaithkhorchfi/demotagpipeline.git']]])
                }
            }
        }
    }

    triggers {
        pollSCM('* * * * *') // Poll SCM for changes every minute, adjust as needed
    }
}
