pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B clean install --fail-never'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                    archiveArtifacts artifacts: '**/target/site/**, **/target/**/*.jar, **/target/**/*.war, **/target/site/apidocs/**', fingerprint: true
                }
            }
        }
        stage('pmd') {
            steps {
                sh 'mvn pmd:pmd'
                sh 'ls -R target/'
            }
            post {
                always {
                    archiveArtifacts artifacts: '**/target/site/pmd.xml', fingerprint: true
                }
            }
        }
        stage('Generate Javadoc') {
            steps {
                sh 'mvn javadoc:jar'
            }
            post {
                always {
                    archiveArtifacts artifacts: '**/target/site/apidocs/**', fingerprint: true
                }
            }
        }
    }
}
