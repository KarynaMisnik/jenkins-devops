// Defines a Declarative Jenkins Pipeline.
pipeline {
    // Runs the pipeline on any available Jenkins agent.
    agent any

    // Defines the external tools required by the pipeline.
    tools {
        jdk 'JDK21'
        maven 'Maven3'
    }

    // Prevents duplicate checkout of the source code.
    options {
        skipDefaultCheckout()
    }

    stages {
        // Stage 1: Retrieves the source code from GitHub.
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        // Stage 2: Verifies that Java, Maven and Git are available.
        stage('Check Tools') {
            steps {
                sh 'java -version'
                sh 'mvn -version'
                sh 'git --version'
            }
        }

        // Stage 3: Compiles, tests and packages the application.
        stage('Build and Test') {
            steps {
                // Runs Maven in batch mode.
                // clean removes files from previous builds.
                // verify compiles, tests, packages and verifies the application.
                sh 'mvn -B clean verify'
            }
        }

        // Stage 4: Archives the generated JAR files in Jenkins.
        stage('Archive Application') {
            steps {
                archiveArtifacts(
                    artifacts: 'target/*.jar',
                    fingerprint: true
                )
            }
        }
    }

    // Defines actions performed after the stages finish.
    post {
        always {
            // Collects and publishes JUnit test results.
            junit(
                testResults: 'target/surefire-reports/*.xml',
                allowEmptyResults: true
            )
        }

        success {
            echo 'Room Management build completed successfully!'
        }

        failure {
            echo 'Room Management build failed. Check the Jenkins console logs.'
        }
    }
}