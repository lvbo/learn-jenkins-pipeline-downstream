pipeline {
  agent any

  parameters {
    choice(name: 'CHOICE', choices: "dev\ntest", description: '请选择要部署的环境')
  }

  environment {
    __GITHUB_ACCESS_KEY = credentials('my_github')
    __GITLAB_USERPWD_PAIR = credentails('gitlab-userpwd-pair')
  }
  
  stages {
    stage("downstream") {
      steps {
        sh "echo downstream"
        withCredentials([string(credentialsId: 'secretText', variable: 'varName')]) {
            echo "${varName}"
        }
        withCredentials([usernamePassword(credentialsId: 'gitlab-userpwd-pair', usernameVariable: 'username', passwordVariable: 'password')]) {
            echo "${username}, ${password}"
        }
        echo "${__GITHUB_ACCESS_KEY}  ${__GITLAB_USERPWD_PAIR_USR}   ${__GITLAB_USERPWD_PAIR_PSW}"
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
