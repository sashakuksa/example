pipeline {
    agent any
    environment {
      GO_ENV = 'development'
    }
stages {
      stage('Checkout') {
        steps {
          git branch: 'master', url: 'https://github.com/sashakuksa/example'
      }
}
stage('Install Dependencies') {
steps {
sh 'go mod tidy'
}
}
stage('Build') {
steps {
sh 'go build -o myapp'
}
}
stage('Test') {
steps {
sh 'go test -v ./... | tee reports/test-results.txt'
}
post {
always {
junit 'reports/test-results.xml'
archiveArtifacts artifacts: 'reports/**', allowEmptyArchive: true
}
}
}
stage('Deploy') {
when {
branch 'master'
}
environment {
GO_ENV = 'production'
}
steps {
sh './deploy-prod.sh'
}
}
stage('Deploy1') {
when {
branch 'dev'
}
steps {
sh './deploy-dev.sh'
}
}
}
post {
always {
echo 'Pipeline finished.'
}
success {
mail to: 'team@example.com',
subject: "SUCCESS: ${currentBuild.fullDisplayName}",
body: "Good news! The build ${currentBuild.fullDisplayName} completed successfully."
}
failure {
mail to: 'team@example.com',
subject: "FAILURE: ${currentBuild.fullDisplayName}",
body: "Bad news! The build ${currentBuild.fullDisplayName} failed. Please check the Jenkins logs f more details."
}
}
}
