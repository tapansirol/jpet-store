node {
  stage('SCM') {
    git 'https://github.com/tapansirol/jpet-store.git'
  }
  stage('SonarQube analysis') {
      // requires SonarQube Scanner 2.8+
    def scannerHome = tool 'sonar-new';
    withSonarQubeEnv('sonar-new') {
      // requires SonarQube Scanner for Maven 3.2+
      sh "${scannerHome}/bin/sonar-scanner"
    }
  }
}
