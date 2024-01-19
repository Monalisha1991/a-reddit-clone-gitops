pipeline {
    agent any
    environment {
          APP_NAME = "reddit-clone-pipeline"
    }
    stages {
         stage("Cleanup Workspace") {
             steps {
                cleanWs()
             }
         }
         stage("Checkout from SCM") {
             steps {
                     git branch: 'main', credentialsId: 'github', url: 'https://github.com/Monalisha1991/a-reddit-clone-gitops'
             }
         }
         stage("Update the Deployment Tags") {
            steps {
                sh """
                    cat deployment.yaml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                    cat deployment.yaml
                """
            }
         }
        stage("Push the changed deployment file to GitHub") {
            steps {
        withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
            sh """
                git config --global user.name "Monalisha1991"
                git config --global user.email "mbehera.16051991@gmail.com"
                git add deployment.yaml
                git commit -m "Updated Deployment Manifest"
                git push
            """
        }
    }
}
    }
}
