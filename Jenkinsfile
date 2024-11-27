@Library('GlobalJenkinsLibrary@2') _

pipeline {

    agent { label 'ubuntu_java-11_docker-20_checkmarx' }
    environment {
        SCANNER_HOME=tool 'SonarQube 3.3'
        BUILD_VERSION = sh(script: 'echo "b${BUILD_ID}_$(date +%Y%m%d)"', returnStdout: true).trim()
        SONAR_BRANCH = "${GIT_BRANCH.replaceFirst(/^.*\//, '')}"
        COMMIT_ID = sh(script: 'echo "${GIT_COMMIT}"', returnStdout: true).trim()
        CURRENT_VERSION = sh(script: 'echo "${env}-${GIT_COMMIT}"', returnStdout: true).trim()
        IMAGE_VERSION = sh(script: 'echo "registry.tools.3stripes.net/eft-wm-ecommercelite/${deployment}"', returnStdout: true).trim()
    }

    stages {
        stage('Compile & Build') {
            steps {
                echo "Compiling"
            }

        }

        stage('Tagging') {
            steps {
            echo "Tagging"
            script{
                BUILD_NUMBER = env.BUILD_NUMBER  // Jenkins provides a build number
                    TAG_NAME = "${VERSION}-build-${BUILD_NUMBER}"
                    echo "Creating tag: ${TAG_NAME}"

                    // Configure git (ensure Jenkins can push)
                    //sh 'git config --global user.email "youremail@example.com"'
                    //sh 'git config --global user.name "Your Name"'

                    // Create and push the tag
                    sh """
                    git tag ${TAG_NAME}
                    git push origin ${TAG_NAME}
                    """
            }
            }
        }

        stage('Deploy') {
          agent { label 'ubuntu_kubectl-1.23' }
            steps {
             echo "Deployment"
            }
        }
    }
        post {
            always {
               echo "Clean up in post work space"
               cleanWs()
            }
        }
}
