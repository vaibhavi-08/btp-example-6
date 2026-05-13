pipeline {
    agent any

    environment {
        // Defining a virtual environment directory
        VENV = 'venv'
    }

    stages {
        stage('Checkout') {
            steps {
                // Pulls the code from the specified repository
                checkout scm
            }
        }

        stage('Setup') {
            steps {
                script {
                    echo 'Setting up Python environment...'
                    // Create a virtual environment and install dependencies
                    sh """
                        python3 -m venv ${VENV}
                        . ${VENV}/bin/activate
                        pip install --upgrade pip
                    """
                    
                    // Conditionally install requirements if the flag is true
                    if ("true" == "true") {
                        sh ". ${VENV}/bin/activate && pip install -r requirements.txt"
                    }
                }
            }
        }

        stage('Quality') {
            steps {
                script {
                    // Flake8 is set to false in config, so we skip or run a simple placeholder
                    echo 'Quality check (Flake8) is disabled in configuration. Skipping...'
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    echo 'Building the Python project...'
                    // For Python, "building" often involves compiling byte-code or packaging
                    sh ". ${VENV}/bin/activate && python3 -m compileall ."
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Tests are set to false in config
                    echo 'Tests are disabled in configuration. Skipping...'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo 'Deploying application...'
                    // Add your specific deployment commands here
                    // e.g., rsync to a server or restarting a systemd service
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up workspace...'
            // Optional: delete the virtualenv after the run
            // sh "rm -rf ${VENV}"
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check the logs.'
        }
    }
}