pipeline {
    agent any

    stages {
        stage('Get Git Tag') {
            steps {
                script {
                env.GIT_TAG = sh(returnStdout: true, script: 'git tag --points-at HEAD | awk \'NR == 1 {print}\'').trim()
                echo "env.GIT_TAG=${env.GIT_TAG}"
                } // script
            } // steps
        }
        stage('Check if tag is pushed') {
            when {
                expression { 
                    return env.BRANCH_NAME == 'develop' && env.GIT_TAG != ''
                }
            }

            steps {
                echo "Tag pushed on develop branch: ${env.GIT_TAG}"
                // Add your build steps here
            }
        }

        stage('Clone Code') {
            when {
                allOf{
                    branch "develop"
                    not{
                        equals expected: "" actual: env.GIT_TAG
                    }

                }
                expression { 
                    return env.BRANCH_NAME == 'develop' && env.GIT_TAG != ''
                }
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
