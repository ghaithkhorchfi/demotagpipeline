pipeline {
    agent any

    stages {
        stage("check commit in develop branch"){
            steps{
                script {
                    echo "${env.GIT_COMMIT}"
                    env.EXIST = sh(returnStdout: true, script: "git branch -a --contains ${env.GIT_COMMIT} | grep develop").trim()
                    echo "${env.EXIST}"
                    env.GET_TAG = sh(returnStdout: true, script: " git describe --tags ${env.GIT_COMMIT}").trim()
                    echo "${env.EXIST} : ${env.GET_TAG} commitId ${env.GIT_COMMIT}"
                } //script
            }//steps
        }
    stage("test condition"){
       when{
           expression{
            return env.EXIST.matches('.*develop*') && env.GET_TAG.matches('.*-rc')
            }
        }
            steps{
                echo "yes done hello final"
            }//steps
        }
        stage('Get Git Tag') {
            steps {
                script {
                env.TAG_NAME = sh(returnStdout: true, script: 'git tag --points-at HEAD | awk \'NR == 1 {print}\'').trim()
                echo "env.TAG_NAME=${env.TAG_NAME}"
                sh 'git checkout develop'
                echo "branch name ${env.BRANCH_NAME}"
                echo "success testing"
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
                tag "*-rc"
            }

            steps {
                echo "Tag pushed on develop branch: ${env.TAG_NAME}"
                // Add your build steps here
            }
        }

    }
}