pipeline {
    agent any

    stages {
        stage('Test') {
            steps {
                script {
                    def changedFile = currentBuild.changeSets
                        .collectMany { it.items }
                        .collectMany { it.affectedFiles }
                        .collect { it.path }
                        .find { true }

                    if (!changedFile) {
                        error "File not found"
                    }

                    echo changedFile

                    sh "pwd"
                    sh "ls -R ."
                    sh "make sync-product-mappings FILE='${changedFile}'"
                }
            }
        }
    }
}
