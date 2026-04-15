pipeline {
    agent any

    stages {

        stage('Debug Info') {
            steps {
                script {
                    echo "Branch actual: ${env.BRANCH_NAME}"
                    echo "PR source: ${env.CHANGE_BRANCH}"
                    echo "PR target: ${env.CHANGE_TARGET}"
                }
            }
        }

        stage('Detect Changes') {
            when {
                changeRequest()
            }
            steps {
                script {

            sh "git fetch origin +refs/heads/*:refs/remotes/origin/*"

            def changedFiles = sh(
                script: "git diff --name-only origin/${env.CHANGE_TARGET}...HEAD",
                returnStdout: true
            ).trim().split("\n")

            echo "Archivos cambiados:"
            changedFiles.each { echo it }

            def services = ["incidente","inventario","monitoreo"]

            def affected = services.findAll { service ->
                changedFiles.any { it.startsWith(service + "/") }
            }

            echo "Servicios afectados: ${affected}"
                }
            }
        }
    }
}