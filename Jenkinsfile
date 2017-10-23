pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo "Continuous Integration on branch ${env.BRANCH_NAME}"
        echo 'Build libraries'
        echo 'Test deployment'
        echo 'Run unittests'
      }
    }
    stage('Deploy') {
      steps {
        echo 'Pack Images'
      }
    }
    stage('Regression') {
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
          when { not { expression { BRANCH_NAME ==~ /^feature.*/ } } }
          steps {
            echo 'Check performance on load'
          }
        }
        stage('Stability') {
          when { not { expression { BRANCH_NAME ==~ /^feature.*/ } } }
          steps {
            echo 'Run chaos monkeys'
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
        echo 'Production deployment'
      }
    }
  }
}
