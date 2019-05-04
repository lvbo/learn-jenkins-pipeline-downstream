pipeline {
  agent any

  parameters {
    choice(name: 'CHOICE', choices: "dev\ntest", description: '请选择要部署的环境')
  }
  
  stages {
    stage("downstream") {
      steps {
        sh "echo downstream"
      }
    }

    stage('deploy test') {
        when {
            expression {
                return params.CHOICE == 'test'
            }
        }
        steps {
            echo "deploy to test"
        }
    }

    stage('deploy dev') {
        when {
            expression {
                return params.CHOICE == 'dev'
            }
        }
        steps {
            echo "deploy to dev"
        }
    }
  }
}
