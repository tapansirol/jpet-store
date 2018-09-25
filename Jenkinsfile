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
			SONAR_USER_HOME=/opt/bitnami/jenkins/.sonar
			sh "${mvnHome}/bin/mvn sonar:sonar"
		}
	}
// stage ("running appscan on cloud"){
//        appscan application: '13a06581-eb2c-4b1f-8002-6722126ae44e', credentials: 'ASOC_Staging', failBuild: true, failureConditions: [failure_condition(failureType: 'high', threshold: 20)], name: 'JPS_test', scanner: static_analyzer('C:\\Users\\kalra_m\\eclipse-workspace-latest\\jpetstore-6'), type: 'Static Analyzer', wait: true
 // }
  stage('publish artificats to ucd'){
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
                pushVersion: '${BUILD_NUMBER}',
                baseDir: 'workspace/jpetstore/target',
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
            	deployEnv: 'Test',
            	deployProc: 'Deploy Jenkins',
            	createProcess: [
                	$class: 'com.urbancode.jenkins.plugins.ucdeploy.ProcessHelper$CreateProcessBlock',
                	processComponent: 'Deploy'
            	],
            	deployVersions: 'jenkins-jpet-component:${BUILD_NUMBER}',
		//deployVersions: 'SNAPSHOT=Base Configuration',
            	deployOnlyChanged: false
        ]
    ])
 } 
}

