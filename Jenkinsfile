pipeline {
  agent any

  environment {
    LABS = credentials('labcreds')
    PIPENV_HOME = '/bitnami/jenkins/home/.local/bin'
  }

  stages {
    stage('Build') {
      steps {
        sh '''
          curl -sS https://bootstrap.pypa.io/get-pip.py | python3.11
          ~/.local/bin/pip install --user pipenv
          $PIPENV_HOME/pipenv --rm || true
          $PIPENV_HOME/pipenv install
        '''
      }
    }

    stage('Test') {
      steps {
        sh '''
          $PIPENV_HOME/pipenv run pytest
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
