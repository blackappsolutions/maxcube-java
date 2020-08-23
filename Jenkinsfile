pipeline {
    agent any

    tools {
        maven 'Maven 3.6.3'
        jdk 'jdk8'
    }

    stages {
        stage('Build') {
            steps {
                // git 'https://github.com/blackappsolutions/maxcube-java.git'
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
