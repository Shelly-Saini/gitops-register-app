pipeline {
    agent { label 'Jenkins-Agent' }

    environment {
        APP_NAME = "register-app"
    }

    stages {

        stage('Cleanup Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout GitOps Repository') {
            steps {
                git branch: 'main',
                    credentialsId: 'github',
                    url: 'https://github.com/Shelly-Saini/gitops-register-app.git'
            }
        }

        stage('Update Deployment Manifest') {
            steps {
                sh """
                sed -i 's#image: .*#image: shelly1230897/${APP_NAME}:${IMAGE_TAG}#g' deployment.yaml
                cat deployment.yaml
                """
            }
        }

        stage('Commit Changes') {
            steps {
                sh """
                git config --global user.name "Shelly-Saini"
                git config --global user.email "shellysaini445@gmail.com"

                git add deployment.yaml
                git commit -m "Updated Deployment Manifest" || true
                """
            }
        }

        stage('Push Changes') {
            steps {
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                    sh "git push origin main"
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
