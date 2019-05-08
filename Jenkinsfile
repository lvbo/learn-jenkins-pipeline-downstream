pipeline {
  agent any

  parameters {
    choice(name: 'CHOICE', choices: "dev\ntest", description: '请选择要部署的环境')
  }

  environment {
    __GITHUB_ACCESS_KEY = credentials('my_github')
    __GITLAB_USERPWD_PAIR = credentials('gitlab-userpwd-pair')
    nexusRawusernamePassword = credentials('nexusRaw')
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
        sh "curl --user '${nexusRawusernamePassword}' --upload-file ./README.md http://192.168.1.55:8081/repository/raw-example/${BUILD_NUMBER}/README.md"
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
