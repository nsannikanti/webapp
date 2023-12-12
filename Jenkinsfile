pipeline {
    // agent { label 'maven-label' }
    agent { label 'aws-label' }

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "maven-3.9.6"
    }

    stages {
        stage('Run Tests') {
            steps {
                checkout scm
                sh "mvn -Dmaven.test.failure.ignore=true -s settings.xml clean deploy"
            }
            post {
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
        stage('SonarQube Analysis') {
            steps {
              withSonarQubeEnv('sonar') {
                  sh "mvn clean verify sonar:sonar -Dsonar.projectKey=myapp"
              }
            }
        }
    }
}