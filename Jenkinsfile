pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "M3"
    }

    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/MaximeStepkowski/HelloWorldMaven.git'

                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true clean install"

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean install"
            }

            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    withCredentials([usernamePassword(credentialsId: '5f76188b-50e0-42f3-813b-c0ce18feecf2', 
                                         passwordVariable: 'GIT_TOKEN', 
                                         usernameVariable: 'GIT_USER')]) {
                        sh "git config user.email 'jenkins-ci@domain.com'"
                        sh "git config user.name 'Jenkins CI User'"
                        sh "git tag v2.${BUILD_NUMBER}"
                        sh "git push https://${GIT_USER}:${GIT_TOKEN}@github.com/MaximeStepkowski/HelloWorldMaven.git v2.${BUILD_NUMBER}"
                    }
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
    }
}
