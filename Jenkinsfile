node {
  stage('SCM') {
    git 'https://github.com/tapansirol/jpet-store.git'
  }
  stage('SonarQube analysis') {
    withSonarQubeEnv('sonar-new') {
      // requires SonarQube Scanner for Maven 3.2+
      sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.2:sonar'
    }
  }
}
