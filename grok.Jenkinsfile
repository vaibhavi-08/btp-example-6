pipeline {
    agent any
    
    tools {
        python 'Python3'  // Ensure Python is configured in Jenkins Global Tool Configuration
    }
    
    environment {
        VENV_DIR = 'venv'
    }
    
    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code...'
                checkout scm
                // Or explicitly: git url: 'https://github.com/vaibhavi-08/btp-example-6', branch: 'master'
            }
        }
        
        stage('Setup') {
            steps {
                echo 'Setting up Python environment and dependencies...'
                sh '''
                    python3 -m venv ${VENV_DIR}
                    . ${VENV_DIR}/bin/activate
                    python -m pip install --upgrade pip
                    if [ -f requirements.txt ]; then
                        pip install -r requirements.txt
                    fi
                '''
            }
        }
        
        stage('Build') {
            steps {
                echo 'Building/Compiling Python project...'
                sh '''
                    . ${VENV_DIR}/bin/activate || true
                    # Simple build step - compile bytecode or run basic checks
                    python -m compileall app/
                    echo "Build completed successfully"
                '''
            }
        }
        
        // Quality stage skipped as flake8: false
        
        // Test stage skipped as tests: false
        
        stage('Deploy') {
            steps {
                echo 'Deploying Python application...'
                sh '''
                    . ${VENV_DIR}/bin/activate || true
                    echo "Running the application..."
                    python app/main.py
                '''
                echo 'Deployment completed (local execution for this simple script project)'
            }
        }
    }
    
    post {
        always {
            echo 'Cleaning up workspace...'
            cleanWs()
        }
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}