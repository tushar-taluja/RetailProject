pipeline {
  agent any

  environment {
    LABS = credentials('labcreds')
  }

  stages {
    stage('Build') {
      steps {
        sh '''
          sudo apt-get update
          sudo apt-get install -y pipx
          pipx ensurepath
          pipx install pipenv || true
          ~/.local/bin/pipenv --rm || true
          ~/.local/bin/pipenv install
        '''
      }
    }

    stage('Test') {
      steps {
        sh '''
          ~/.local/bin/pipenv run pytest
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
