node {
  stage('SCM') {
    checkout scm
  }
  stage('SonarQube Analysis') {
    def mvn = tool 'default-maven';
    withSonarQubeEnv() {
      sh "${mvn}/bin/mvn clean verify sonar:sonar -Dsonar.projectKey=cloudbees-replication-test-project -Dsonar.projectName='cloudbees-replication-test-project' -Dsonar.analysis.buildTag=${env.BUILD_TAG}"
    }
  }}

stage("Quality Gate"){
   // timeout(time: 10, unit: 'SECONDS') {
      def qg = waitForQualityGate() 
      if (qg.status != 'OK') {
        error "Pipeline aborted due to quality gate failure: ${qg.status}"
      }
   // }
}
