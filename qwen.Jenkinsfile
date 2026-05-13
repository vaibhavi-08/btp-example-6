pipeline {
    agent any

    environment {
        PYTHON_VERSION = '3.9'
        VENV_PATH = 'venv'
    }

    stages {
        // Stage 1: Checkout source code from GitHub
        stage('Checkout') {
            steps {
                checkout scm
                echo "✅ Checked out repository: ${env.GIT_URL}"
            }
        }

        // Stage 2: Setup Python environment and install dependencies
        stage('Setup') {
            steps {
                script {
                    // Create virtual environment
                    sh "python${PYTHON_VERSION} -m venv ${VENV_PATH}"
                    
                    // Activate and install dependencies
                    sh """
                        . ${VENV_PATH}/bin/activate
                        python -m pip install --upgrade pip
                        pip install -r requirements.txt
                        echo "✅ Dependencies installed from requirements.txt"
                    """
                }
            }
        }

        // Stage 3: Quality checks (flake8) - SKIPPED per configuration
        // flake8: "false" → This stage is disabled
        /*
        stage('Quality') {
            steps {
                script {
                    sh """
                        . ${VENV_PATH}/bin/activate
                        flake8 . --count --select=E9,F63,F7,F82 --show-source --statistics
                        flake8 . --count --exit-zero --max-complexity=10 --max-line-length=127 --statistics
                    """
                }
            }
        }
        */

        // Stage 4: Build - Compile Python bytecode and validate syntax
        stage('Build') {
            steps {
                script {
                    sh """
                        . ${VENV_PATH}/bin/activate
                        python -m compileall -q .
                        echo "✅ Build completed: Python syntax validation passed"
                    """
                }
            }
        }

        // Stage 5: Test execution - SKIPPED per configuration
        // tests: "false" → This stage is disabled
        /*
        stage('Test') {
            steps {
                script {
                    sh """
                        . ${VENV_PATH}/bin/activate
                        python -m pytest tests/ --cov=app --cov-report=xml || echo "⚠️ No tests found or tests disabled"
                    """
                }
                post {
                    always {
                        junit 'test-reports/*.xml'
                        publishHTML target: [
                            allowMissing: true,
                            alwaysLinkToLastBuild: true,
                            keepAll: true,
                            reportDir: 'htmlcov',
                            reportFiles: 'index.html',
                            reportName: 'Coverage Report'
                        ]
                    }
                }
            }
        }
        */

        // Stage 6: Deploy - SKIPPED per configuration
        // Dockerfile: "false" → No containerization or deployment configured
        /*
        stage('Deploy') {
            when {
                branch 'master'
            }
            steps {
                script {
                    // Example: Docker build and push (disabled)
                    // sh "docker build -t myapp:${env.BUILD_NUMBER} ."
                    // sh "docker push myapp:${env.BUILD_NUMBER}"
                    echo "ℹ️ Deployment stage disabled - no Dockerfile configured"
                }
            }
        }
        */
    }

    post {
        always {
            // Cleanup virtual environment to save workspace space
            sh "rm -rf ${VENV_PATH}"
            echo "🧹 Workspace cleaned"
        }
        success {
            echo "🎉 Pipeline completed successfully!"
        }
        failure {
            echo "❌ Pipeline failed - check console output for details"
            // Optional: Add notification logic here (email, Slack, etc.)
        }
    }
}