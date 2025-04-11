pipeline {
  agent any

  environment {
    LABS = credentials('labcreds')
    PYTHON = 'python3.11'
    PIPENV_PATH = '.venv/bin/pipenv'
    JAVA_HOME = '/usr/lib/jvm/java-11-openjdk-amd64'
    PATH = "${env.JAVA_HOME}/bin:${env.PATH}:${env.WORKSPACE}/.venv/bin"
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: "${env.BRANCH_NAME}", credentialsId: 'gitcreds', url: 'https://github.com/rahulswami1717/RetailProject.git'
      }
    }

    stage('Build') {
      steps {
        sh '''
          echo "[Build] Setting up Python Virtual Environment"
          $PYTHON -m venv .venv
          .venv/bin/pip install --upgrade pip
          .venv/bin/pip install pipenv
          $PIPENV_PATH install
        '''
      }
    }

    stage('Test') {
      steps {
        sh '''
          echo "[Test] Verifying JAVA_HOME: $JAVA_HOME"
          java -version || echo "Java not found in PATH"
          
          echo "[Test] Running Pytest with Spark Tests"
          $PIPENV_PATH run pytest || exit 1
        '''
      }
    }

    stage('Package') {
      steps {
        sh '''
          echo "[Package] Simulating packaging step (if any)"
          mkdir -p dist
          tar -czf dist/retailproject.tar.gz *.py || true
        '''
      }
    }

    stage('Deploy') {
      steps {
        sh '''
          echo "[Deploy] Simulating deployment step"
          echo "Deploying artifact dist/retailproject.tar.gz to dummy server..."
          # Simulate SCP or GCP CLI etc if needed
        '''
      }
    }
  }

  post {
    always {
      echo 'Cleaning up...'
      sh 'rm -rf .venv dist'
    }
  }
}
