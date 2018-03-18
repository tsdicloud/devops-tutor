pipeline {
  agent any
  stages {
   stage('Build') {
      steps {
        echo "Continuous Integration on branch ${env.BRANCH_NAME}"
        echo 'Code checks' 
        echo 'Build libraries'
        echo 'Run unittests'
      }
    }
   stage('DevQuality') {
      steps {
        echo "Checking ${env.BRANCH_NAME}"
        echo 'Unittests...'
        echo 'Test installation'
        echo 'Smoke test'
      }
    }
    stage('BizRegression') {
      when { not { expression { BRANCH_NAME ==~ /^feature.*/ } } }
      steps {
        echo 'Install '
      }
    }
    stage('NFRegression') {
      when { not { expression { BRANCH_NAME ==~ /^feature.*/ } } }
      parallel {
        stage('Interoperability') {
          steps {
            echo 'Check interfaces/integrations'
          }
        }
        stage('Performance') {
          steps {
            echo 'Check performance on load'
          }
        }
        stage('Stability') {
          steps {
            echo 'Run chaos monkeys'
          }
        }
        stage('RollingUpdate') {
          steps {
            echo 'Install n-1 release from tag'
            echo 'Run rolling update scripts'
            echo 'Calm down phase'
            echo 'Run Biz regression'
          }
        }
      }
    }
    stage('Acceptance') {
      when { not { expression { BRANCH_NAME ==~ /^feature.*/ } } }
      steps {
        input 'Accept for production?'
      }
    }
    stage('Production') {
      when { not { expression { BRANCH_NAME ==~ /^feature.*/ } } }
      steps {
        echo 'Production installation'
      }
    }
  }
}
