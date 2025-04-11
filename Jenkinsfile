pipeline {
  agent any

  environment {
    LABS = credentials('labcreds')
    PIPENV_PATH = '.venv/bin/pipenv'
    PYTHON = 'python3.11'
  }

  stages {
    stage('Build') {
      steps {
        sh '''
          $PYTHON -m venv .venv
          .venv/bin/pip install --upgrade pip
          .venv/bin/pip install pipenv
          $PIPENV_PATH --rm || true
          $PIPENV_PATH install
        '''
      }
    }

    stage('Test') {
      steps {
        sh '''
          $PIPENV_PATH run pytest
        '''
      }
    }

    stage('Package') {
      steps {
        sh 'zip -r retailproject.zip .'
      }
    }

    stage('Deploy') {
      steps {
        sh '''
          sshpass -p $LABS_PSW scp -o StrictHostKeyChecking=no -r . \
          $LABS_USR@g02.itversity.com:/home/itv005857/retailproject
        '''
      }
    }
  }
}
