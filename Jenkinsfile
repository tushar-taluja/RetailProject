pipeline {
  agent any

  environment {
    LABS = credentials('labcreds')
    PIPENV_PATH = '/bitnami/jenkins/home/.local/bin/pipenv'
  }

  stages {
    stage('Build') {
      steps {
        sh '''
          # Install pipenv (safely bypassing system restrictions)
          pip3 install --user --break-system-packages pipenv

          # Remove any old environment
          ${PIPENV_PATH} --rm || true

          # Create new env with correct Python version
          ${PIPENV_PATH} install --python 3.11
        '''
      }
    }

    stage('Test') {
      steps {
        sh '''
          ${PIPENV_PATH} run pytest
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
