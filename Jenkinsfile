pipeline {
  agent none
  stages {
    stage('YAML') {
      agent {
        docker {
          image 'pipelinecomponents/yamllint:latest'
        }

      }
      steps {
        sh 'yamllint .'
      }
    }
    stage('Remark') {
      agent {
        docker {
          image 'pipelinecomponents/remark-lint'
        }

      }
      steps {
        sh 'remark --no-stdout --color --frail --use preset-lint-recommended .'
      }
    }
    stage('Home Assistant Check') {
      parallel {
        stage('home_assistant_stable') {
          agent {
            docker {
              image 'homeassistant/home-assistant:stable'
            }

          }
          steps {
            sh 'mv travis-secrets.yaml secrets.yaml'
            sh 'python -m homeassistant --version'
            sh 'python -m homeassistant --config . --script check_config --info all'
          }
        }
      }
    }
  }
}
