
pipeline {
    agent any
    stages {
        stage ('cloning the repository'){
            steps {
            node ('master'){
                 git 'https://github.com/tapansirol/jpet-store'
            }
        }
        }
	    
	    stage ('Build') {
            steps {
            node ('master'){
                 withMaven(jdk: 'JDK_local', maven: 'MVN_Local') {
      sh 'mvn clean package'
    }
            }
        }
        }
    }
    post { 
        always { 
            echo 'I will always say Hello!'
		 sh '/home/config/hcl-onetest-command.sh'
        }
    }
}
