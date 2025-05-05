pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps {
        checkout([$class: 'GitSCM',
          branches: [[name: '*/main']], 
          userRemoteConfigs: [[
            url: 'https://github.com/alfikriangelo/tugas-vuln-devsecop.git',
            credentialsId: 'github-token' 
          ]]
        ])
      }
    }
    stage('Install Dependencies') {
      steps {
        sh '''
          python3 -m venv venv
          . venv/bin/activate
          pip install --upgrade pip
          pip install bandit
        '''
      }
    }
    stage('SAST Analysis') {
      steps {
        sh '''
          . venv/bin/activate
          bandit -f xml -o bandit-output.xml -r . || true
        '''
        archiveArtifacts artifacts: 'bandit-output.xml'
      }
    }
  }
}
