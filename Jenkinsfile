pipeline {
    agent any
    stages {
        stage('Check for CHANGELOG update') {
            when { expression { env.BRANCH_NAME != 'master' } }
            steps {
                script {
                    // Replace this with your SSH creds for git
                    sshagent(['git@github.com:mohans07/rollback.git']) {
                        List<String> sourceChanged = sh(returnStdout: true, script: "git diff --name-only origin/master..origin/${env.BRANCH_NAME}").split()
                        def isSourceChanged = false
                        def isChangelogUpdated = false
                        for (int i = 0; i < sourceChanged.size(); i++) {
                            if (sourceChanged[i].contains("src")) {
                                isSourceChanged = true
                            }
                            if (sourceChanged[i].contains("CHANGELOG")) {
                                isChangelogUpdated = true
                            }
                        }
                        // Changelog not updated after changing source, fail build and notify
                        if (isSourceChanged && !isChangelogUpdated) {
                            error("Source files changed but CHANGELOG was not updated!")
                        }
                    }
                }
            }
        }   
    }    
}
