node{
  stage ('cloning the repository'){
      git 'https://github.com/tapansirol/jpet-store'
  }
	
  //stage('SonarQube analysis') {
    // requires SonarQube Scanner 2.8+
    //def scannerHome = tool 'sonar-new';
    //withSonarQubeEnv('My SonarQube Server') {
     // sh "${scannerHome}/bin/sonar-scanner"
    //}
  //}
  stage ('Build') {
      withMaven(jdk: 'JDK_local', maven: 'MVN_Local') {
      sh 'mvn clean package'
    }
  }
	
	stage('SonarQube Analysis'){
		def mvnHome = tool name : 'MVN_Local', type:'maven'
		withSonarQubeEnv('sonar-server'){
			 //"SONAR_USER_HOME=/opt/bitnami/jenkins/.sonar ${mvnHome}/bin/mvn sonar:sonar"
			sh  "${mvnHome}/bin/mvn sonar:sonar"
		}
	}
stage ("Appscan"){
	//sleep 40
     //appscan application: '17969f05-19dd-4143-b7e2-c52a3336db18', credentials: 'asoc', failBuild: true, failureConditions: [failure_condition(failureType: 'high', threshold: 4)], name: '17969f05-19dd-4143-b7e2-c52a3336db185549', scanner: static_analyzer('/var/jenkins_home/jobs/JPetStore-test'), type: 'Static Analyzer', wait: true
	//appscan application: '13a06581-eb2c-4b1f-8002-6722126ae44e', credentials: 'ASOC_Staging', failBuild: true, failureConditions: [failure_condition(failureType: 'high', threshold: 20)], name: 'JPS_test', scanner: static_analyzer('C:\\Users\\kalra_m\\eclipse-workspace-latest\\jpetstore-6'), type: 'Static Analyzer', wait: true
	 //appscan application: '17969f05-19dd-4143-b7e2-c52a3336db18', credentials: 'asoc', failBuild: true, failureConditions: [failure_condition(failureType: 'high', threshold: 20)], name: '17969f05-19dd-4143-b7e2-c52a3336db185549', scanner: static_analyzer('C:\\Program Files (x86)\\Jenkins\\jobs\\JPetStore-Test'), type: 'Static Analyzer', wait: true
//appscan application: 'd25a0655-7bd2-418f-8a34-ca8338e411c0', credentials: Credential for ASOC', name: 'd25a0655-7bd2-418f-8a34-ca8338e411c09964', scanner: static_analyzer(hasOptions: false, target: ''), type: 'Static Analyzer'
//appscan application: 'd25a0655-7bd2-418f-8a34-ca8338e411c0', credentials: 'Credential for ASOC', failBuild: true, failureConditions: [failure_condition(failureType: 'high', threshold: 20)], name: 'd25a0655-7bd2-418f-8a34-ca8338e411c09964', scanner: static_analyzer('/var/jenkins_home/jobs/JPetStore-test'), type: 'Static Analyzer', wait: true

	appscan application: '17969f05-19dd-4143-b7e2-c52a3336db18', credentials: 'Credential for ASOC', failBuild: true, failureConditions: [failure_condition(failureType: 'high', threshold: 20)], name: 'test_07012019', scanner: static_analyzer(hasOptions: false, target: '/var/jenkins_home/jobs/jpetstore'), type: 'Static Analyzer', wait: true
 }
  stage('Publish Artificats to UCD'){
   step([$class: 'UCDeployPublisher',
        siteName: 'ucd-server',
        component: [
            $class: 'com.urbancode.jenkins.plugins.ucdeploy.VersionHelper$VersionBlock',
            componentName: 'jenkins-jpet-component',
            createComponent: [
                $class: 'com.urbancode.jenkins.plugins.ucdeploy.ComponentHelper$CreateComponentBlock',
                componentTemplate: '',
                componentApplication: 'JPetStore'
            ],
            delivery: [
                $class: 'com.urbancode.jenkins.plugins.ucdeploy.DeliveryHelper$Push',
                pushVersion: 'ver${BUILD_NUMBER}',
               // baseDir: '/var/jenkins_home/workspace/JPetStore/target',
		baseDir: '/var/jenkins_home/workspace/jpetstore/target',
                fileIncludePatterns: '*.war',
                fileExcludePatterns: '',
               // pushProperties: 'jenkins.server=Jenkins-app\njenkins.reviewed=false',
                pushDescription: 'Pushed from Jenkins'
            ]
        ]
    ])
	step([$class: 'UCDeployPublisher',
        	siteName: 'ucd-server',
        	deploy: [
            	$class: 'com.urbancode.jenkins.plugins.ucdeploy.DeployHelper$DeployBlock',
            	deployApp: 'JPetStore',
            	deployEnv: 'JPetStore_Dev',
            	deployProc: 'Deploy-JPetStore',
            	createProcess: [
                	$class: 'com.urbancode.jenkins.plugins.ucdeploy.ProcessHelper$CreateProcessBlock',
                	processComponent: 'Deploy'
            	],
            	deployVersions: 'jenkins-jpet-component:ver${BUILD_NUMBER}',
		//deployVersions: 'SNAPSHOT=Base Configuration',
            	deployOnlyChanged: false
        ]
    ])
 }
 
//stage ('HCL One Test') {
//	echo 'Executing HCL One test ... '
//	sh '/var/jenkins_home/onetest/hcl-onetest-command.sh'
  //}

}
