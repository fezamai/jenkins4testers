def COLOR_MAP = ['SUCCESS': 'good', 'FAILURE': 'danger', 'UNSTABLE': 'danger', 'ABORTED': 'danger']

pipeline {
  agent {
    docker {
        image "ruby:alpine"
        args "--network=skynet"
    }
  }
  stages {
    stage("Build") {
      steps {
        sh "chmod +x build/alpine.sh"
        sh "./build/alpine.sh"
        sh "bundle install"
      }
    }
    stage("Test") {
      steps {
        sh "bundle exec cucumber -p ci"
        cucumber failedFeaturesNumber: -1, failedScenariosNumber: -1, failedStepsNumber: -1, fileIncludePattern: '**/*.json', jsonReportDirectory: 'log', pendingStepsNumber: -1, skippedStepsNumber: -1, sortingMethod: 'ALPHABETICAL', undefinedStepsNumber: -1
      }
      post{
        always{
            cucumber failedFeaturesNumber: -1, failedScenariosNumber: -1, failedStepsNumber: -1, fileIncludePattern: '**/*.json', jsonReportDirectory: 'log', pendingStepsNumber: -1, skippedStepsNumber: -1, sortingMethod: 'ALPHABETICAL', undefinedStepsNumber: -1
            slackSend channel: "#curso-jenkins",
                color: COLOR_MAP[currentBuild.currentResult],
                message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD}\n Mais informações acesse: ${env.BUILD_URL}"
        }
      }
    }
  }
}
