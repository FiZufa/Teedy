pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh 'mvn test'
      }
      post {
        always {
          junit 'target/surefire-reports/*.xml'
          archiveArtifacts artifacts: 'target/site/**, target/**/*.jar, target/**/*.war, target/site/apidocs/**', fingerprint: true
        }
      }
  }
}
}
