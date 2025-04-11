pipeline {
    agent any

    environment {
        PYTHON = 'python3.11'
        JAVA_HOME = '/usr/lib/jvm/java-11-openjdk-amd64'
        PIPENV_PATH = '.venv/bin/pipenv'
    }

    options {
        timestamps()
    }

    stages {

        stage('Checkout') {
            steps {
                echo '📦 Checking out code from Git...'
                checkout scm
                sh 'git fetch --all'
                sh 'git reset --hard origin/${BRANCH_NAME}'
                sh 'git log -1 --oneline'
            }
        }

        stage('Setup Environment') {
            steps {
                echo '⚙️ Creating virtual environment and installing dependencies...'
                sh '''
                    ${PYTHON} -m venv .venv
                    .venv/bin/pip install --upgrade pip
                    .venv/bin/pip install pipenv
                    ${PIPENV_PATH} install --dev
                '''
            }
        }

        stage('Run Tests') {
            steps {
                echo '🧪 Running Pytest on Spark tests...'
                sh '''
                    export JAVA_HOME=${JAVA_HOME}
                    export PATH=$JAVA_HOME/bin:$PATH
                    ${PIPENV_PATH} run pytest
                '''
            }
        }

        stage('Package') {
            steps {
                echo '📦 Zipping project for packaging...'
                sh 'zip -r retail_project_package.zip . -x ".venv/*" "*.git*"'
            }
        }

        stage('Deploy') {
            steps {
                echo '🚀 Deployment stage placeholder...'
                // Add your actual deployment logic here
                sh 'echo "Deployed RetailProject build from branch ${BRANCH_NAME}"'
            }
        }
    }

    post {
        success {
            echo '✅ Build, Test & Deploy completed successfully.'
        }
        failure {
            echo '❌ Build or tests failed. Please check logs.'
        }
    }
}
