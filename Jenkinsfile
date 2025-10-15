pipeline {
  agent any

  environment {
    VENV_DIR = ".venv"
    REPORT_DIR = "build/reports"
  }

  options {
    timeout(time: 30, unit: 'MINUTES')
    buildDiscarder(logRotator(numToKeepStr: '10'))
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Setup Python') {
      steps {
        sh '''
          python3 -m venv ${VENV_DIR}
          . ${VENV_DIR}/bin/activate
          python -m pip install --upgrade pip
          pip install -r requirements.txt
        '''
      }
    }

    stage('Run Tests') {
    steps {
        sh '''
            . .venv/bin/activate
            mkdir -p build/reports
            export PYTHONPATH=.
            pytest --junitxml=build/reports/results.xml --cov=. --maxfail=1 -q
        '''
      } 
    }

  }

  post {
    success {
      echo '✅ All tests passed!'
    }
    failure {
      echo '❌ Tests failed — check the test report.'
    }
  }
}
