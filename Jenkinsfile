pipeline {
    agent any

    tools {
        maven 'Maven 3.6.3'
        jdk 'jdk8'
    }

    stages {
        stage('Build') {
            steps {
                script {
                    showChanges()
                }
                sh "mvn -Dmaven.test.failure.ignore=true clean package"
            }

            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results (and archive the jar file).
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    // archiveArtifacts 'target/*.jar'
                }
            }
        }
    }
}

def showChanges() {
    def changeLogSets = currentBuild.changeSets
    echo "changeLogSets.size=${changeLogSets.size()}"
    for (int i = 0; i < changeLogSets.size(); i++) {
        def entries = changeLogSets[i].items
        for (int j = 0; j < entries.length; j++) {
            def entry = entries[j]
            echo "${entry.commitId} by ${entry.author} on ${new Date(entry.timestamp)}: ${entry.msg}"
            def files = new ArrayList(entry.affectedFiles)
            for (int k = 0; k < files.size(); k++) {
                def file = files[k]
                echo "  ${file.editType.name} ${file.path}"
            }
        }
    }
}
