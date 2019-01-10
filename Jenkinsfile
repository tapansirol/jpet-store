pipeline {
	stages {
  stage ('cloning the repository'){
      git 'https://github.com/tapansirol/jpet-store'
  }
	
  stage ('Build') {
      withMaven(jdk: 'JDK_local', maven: 'MVN_Local') {
      sh 'mvn clean package'
    }
  }
	}

post { 
        always { 
            sh '/home/config/hcl-onetest-command'
        }
    }
}
