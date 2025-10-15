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
          . ${VENV_DIR}/bin/activate
          mkdir -p ${REPORT_DIR}
          pytest --junitxml=${REPORT_DIR}/results.xml --cov=. --maxfail=1 -q
        '''
      }
      post {
        always {
          junit 'build/reports/results.xml'
        }
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
