pipeline {
    agent any

    environment {
        // Define environment variables
        HTTPD_PACKAGE = "httpd"
        LOG_PATH = '/var/log/httpd/access_log'  // Path to Apache log file
    }

    stages {
        stage('Update System') {
            steps {
                script {
                    // Update the system before installing Apache
                    sh 'sudo dnf update -y'
                }
            }
        }

        stage('Install Apache HTTP Server') {
            steps {
                script {
                    // Install the Apache HTTP Server
                    sh 'sudo dnf install -y httpd'
                }
            }
        }

        stage('Start Apache Service') {
            steps {
                script {
                    // Start Apache HTTP server
                    sh 'sudo systemctl start httpd'
                }
            }
        }

        stage('Enable Apache on Boot') {
            steps {
                script {
                    // Enable Apache to start on boot
                    sh 'sudo systemctl enable httpd'
                }
            }
        }

        stage('Verify Installation') {
            steps {
                script {
                    // Check if Apache is running
                    sh 'sudo systemctl status httpd'
                }
            }
        }

        stage('Check Apache Logs for Errors') {
            steps {
                script {
                    // Check for 4xx and 5xx errors in the Apache access log
                    def fourXXErrors = sh(script: "grep ' 4' ${LOG_PATH} | wc -l", returnStdout: true).trim()
                    def fiveXXErrors = sh(script: "grep ' 5' ${LOG_PATH} | wc -l", returnStdout: true).trim()

                    // Output the results to Jenkins logs
                    echo "4xx Errors: ${fourXXErrors}"
                    echo "5xx Errors: ${fiveXXErrors}"

                    // Optional: Fail the build if errors exceed a certain threshold
                    if (fourXXErrors.toInteger() > 10) {
                        error("Too many 4xx errors in the Apache logs!")
                    }
                    if (fiveXXErrors.toInteger() > 5) {
                        error("Too many 5xx errors in the Apache logs!")
                    }
                }
            }
        }

        stage('Deployment') {
            steps {
                script {
                    // Perform deployment tasks here (e.g., moving files, configuring services)
                    echo 'Deployment successful!'
                }
            }
        }
    }

    post {
        success {
            echo 'Apache HTTP Server has been successfully installed, started, and deployed.'
        }
        failure {
            echo 'There was an error during the installation or deployment.'
        }
    }
}
