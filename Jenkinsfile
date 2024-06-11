node {
  stage('SCM') {
    checkout scm
  }
  stage('SonarQube Analysis') {
    def mvn = tool 'default-maven';
    withSonarQubeEnv() {
      sh "${mvn}/bin/mvn clean verify sonar:sonar -Dsonar.projectKey=cloudbees-replication-test-project -Dsonar.projectName='cloudbees-replication-test-project'"
    }
  }
}

stage("Quality Gate"){
     if (BRANCH_NAME == "develop") {
       echo "In 'develop' branch, skip."
     }
     else { // this is a PR build, fail on threshold spill
       def qualitygate = waitForQualityGate()
       if (qualitygate.status != "OK") {
         error "Pipeline aborted due to quality gate coverage failure: ${qualitygate.status}"
       } 
     }
  
    timeout(time: 10, unit: 'SECONDS') {
      def qg = waitForQualityGate() 
      if (qg.status != 'OK') {
        error "Pipeline aborted due to quality gate failure: ${qg.status}"
      }
    }
}
