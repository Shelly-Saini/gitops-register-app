pipeline {
    agent { label "Jenkins-Agent" }

    environment {
        APP_NAME = "register-app-pipeline"
    }

    stages {

        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
            steps {
                git branch: 'main',
                    credentialsId: 'github',
                    url: 'https://github.com/Shelly-Saini/gitops-register-app'
            }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh '''
                    echo "Deployment Manifest Before Update"
                    cat deployment.yaml

                    sed -i "s|${APP_NAME}:.*|${APP_NAME}:${IMAGE_TAG}|g" deployment.yaml

                    echo "Deployment Manifest After Update"
                    cat deployment.yaml
                '''
            }
        }

        stage("Push Updated Deployment Manifest") {
            steps {
                sh '''
                    git config --global user.name "Shelly-Saini"
                    git config --global user.email "shellysaini445@gmail.com"

                    git add deployment.yaml

                    git commit -m "Updated Deployment Manifest" || true
                '''

                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                    sh 'git push https://github.com/Shelly-Saini/gitops-register-app main'
                }
            }
        }
    }
}
