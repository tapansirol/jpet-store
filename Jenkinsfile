node{
  stage ('cloning the repository'){
      git 'https://github.com/tapansirol/jpet-store'
  }
  stage ('Build') {
      withMaven(jdk: 'JDK_local', maven: 'MVN_Local') {
      bat 'mvn clean package'
    }
  }
   step([$class: 'UCDeployPublisher',
        siteName: 'ucd-server',
        component: [
            $class: 'com.urbancode.jenkins.plugins.ucdeploy.VersionHelper$VersionBlock',
            componentName: 'jenkins-test-component',
            createComponent: [
                $class: 'com.urbancode.jenkins.plugins.ucdeploy.ComponentHelper$CreateComponentBlock',
                componentTemplate: '',
                componentApplication: 'Jenkins-app'
            ],
            delivery: [
                $class: 'com.urbancode.jenkins.plugins.ucdeploy.DeliveryHelper$Push',
                pushVersion: '${BUILD_NUMBER}',
                baseDir: 'workspace\\jpet-store-test-tapan\\target',
                fileIncludePatterns: '*.war',
                fileExcludePatterns: '',
                //pushProperties: 'jenkins.server=Local\njenkins.reviewed=false',
                //pushDescription: 'Pushed from Jenkins'
            ]
        ]
    ])
	step([$class: 'UCDeployPublisher',
        	siteName: 'ucd-server',
        	deploy: [
            	$class: 'com.urbancode.jenkins.plugins.ucdeploy.DeployHelper$DeployBlock',
            	deployApp: 'Jenkins-app',
            	deployEnv: 'Test',
            	deployProc: 'Deploy Jenkins',
            	createProcess: [
                	$class: 'com.urbancode.jenkins.plugins.ucdeploy.ProcessHelper$CreateProcessBlock',
                	processComponent: 'Deploy'
            	],
            	deployVersions: 'jenkins-test-component:${BUILD_NUMBER}',
            	deployOnlyChanged: false
        ]
    ])
  
}

