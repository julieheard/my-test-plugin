node {
  stage('SCM') {
    checkout scm
  }
  stage('SonarQube Analysis') {
    def mvn = tool 'default-maven';
    def scannerHome = tool 'sq-scanner';
    withSonarQubeEnv() {
      //sh "${mvn}/bin/mvn clean verify sonar:sonar -Dsonar.projectKey=testProject -Dsonar.projectName='testProject'"
      sh "${scannerHome}/bin/sonar-scanner"
    }
  }
}

stage('sonarqube') {

    withSonarQubeEnv('cloudbees') {
      sh "${scannerHome}/bin/sonar-scanner"
    }
  }
}