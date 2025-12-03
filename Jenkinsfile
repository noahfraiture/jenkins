pipeline {
    agent any

    parameters {
        choice(
            name: 'EVENT',
            choices: ['COMMIT', 'OTHER'],
            description: 'Type of event that triggered this build'
        )
    }

    stages {
        stage('Test') {
            when {
                expression { return params.EVENT == 'COMMIT' }
            }
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

                    sh "make sync-product-mappings FILE='${changedFile}'"
                }
            }
        }
    }
}
