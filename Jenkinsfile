/*
 See the documentation for more options:
 https://github.com/jenkins-infra/pipeline-library/

buildPlugin(
  forkCount: '1C', // run this number of tests in parallel for faster feedback.  If the number terminates with a 'C', the value will be multiplied by the number of available CPU cores
  useContainerAgent: true, // Set to `false` if you need to use Docker for containerized tests
  configurations: [
    [platform: 'linux', jdk: 21],
    [platform: 'windows', jdk: 17],
])
*/
node {
  stage('SCM') {
    checkout scm
  }
  stage('SonarQube Analysis') {
    def mvn = tool 'Default Maven';
    withSonarQubeEnv('my-sonarqube-instance') {
      sh "${mvn}/bin/mvn clean install"
      sh "${mvn}/bin/mvn verify sonar:sonar -Dsonar.projectKey=testProject -Dsonar.projectName='testProject' -Dsonar.login=sqp_586844c9aa35880fa4e86089178aa0e4a5109823"
    }
  }
}