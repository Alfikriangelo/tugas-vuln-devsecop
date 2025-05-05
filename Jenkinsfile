pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps {
        checkout([$class: 'GitSCM',
          branches: [[name: '*/main']], // Ganti 'main' jika branch kamu 'master'
          userRemoteConfigs: [[
            url: 'https://github.com/alfikriangelo/tugas-vuln-devsecop.git',
            credentialsId: 'github-token' // <- pastikan ID ini sesuai dengan yang di Jenkins
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
        recordIssues tools: [bandit(pattern: 'bandit-output.xml')]
      }
    }
  }
}
