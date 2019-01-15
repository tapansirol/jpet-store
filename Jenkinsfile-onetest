node{
  stage ('cloning the repository'){
      git 'https://github.com/tapansirol/jpet-store'
  }
	
 
  stage ('Build') {
      withMaven(jdk: 'JDK_local', maven: 'MVN_Local') {
      sh 'mvn clean package'
    }
  }
	
 stage ('One Test') {
 	echo 'I will always say Hello!'
	sh '/var/jenkins_home/hcl-onetest-command.sh'
  }

}
