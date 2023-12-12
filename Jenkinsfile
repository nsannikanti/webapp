pipeline {
    // agent { label 'maven-label' }
    agent { label 'aws-label' }

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "maven-3.9.6"
    }

    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                checkout scm

                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true -settings.xml clean deploy"

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }

            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
    }
}