pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'Build libraries'
        echo 'Test deployment'
        echo 'Run unittests'
      }
    }
    stage('Deploy') {
      when { not { branch 'feature' } }
      steps {
        echo 'Pack Images'
      }
    }
    stage('Regression') {
      when { not { branch 'feature' } }
      parallel {
        stage('Business') {
          steps {
            echo 'Check business functionality'
          }
        }
        stage('Integration') {
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
      }
    }
    stage('Acceptance') {
      when { not { branch 'feature' } }
      steps {
        input 'Accept for production?'
      }
    }
    stage('Production') {
      when { not { branch 'feature' } }
      steps {
        echo 'Production deployment'
      }
    }
  }
}
