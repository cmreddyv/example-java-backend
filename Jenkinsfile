//Adding the Github Poll SCM
properties([pipelineTriggers([githubPush()])])

pipeline {
    agent any
    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "M3"
    }

    stages {    
        stage('Checkout SCM') {
            steps {
                checkout([
                        $class: 'GitSCM',
                        branches: [[name: 'master']],
                        userRemoteConfigs: [[
                        url: 'https://github.com/cmreddyv/example-java-backend.git',
                        credentialsId: '',
                        ]]
                        ])
                    }
                }

        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/cmreddyv/example-java-backend.git'

                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true clean package"


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
        stage("publish to s3") {
          steps{
              sh 'aws s3 cp target/*.jar s3://chandrajenkins'
          }
}
        stage('Test') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/jglick/simple-maven-project-with-tests.git'

                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true clean package"

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }
        }
        stage('Deploy') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/jglick/simple-maven-project-with-tests.git'

                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true clean package"

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


