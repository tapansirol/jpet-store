node{
  stage ('cloning the repository'){
      git 'https://github.com/tapansirol/jpet-store'
  }
  stage ('Build') {
      withMaven(jdk: 'JDK_local', maven: 'MVN_Local') {
      bat 'mvn clean package'
    } +96
  }
stage ('Push to UCD...') {
       step([$class: 'UCDeployPublisher',
            siteName: 'Docker IBM UrbanCode Deploy Server',
            component: [
                $class: 'com.urbancode.jenkins.plugins.ucdeploy.VersionHelper$VersionBlock',
                componentName: 'UCD - Pipeline',
                createComponent: [
                    $class: 'com.urbancode.jenkins.plugins.ucdeploy.ComponentHelper$CreateComponentBlock'
                ],
                delivery: [
                    $class: 'com.urbancode.jenkins.plugins.ucdeploy.DeliveryHelper$Push',
                    pushVersion: '${BUILD_NUMBER}',
                    baseDir: '.',
                    fileIncludePatterns: '/var/jenkins_home/workspace/datapower-pipeline/dist/*',
                    fileExcludePatterns: '',
                    pushProperties: 'jenkins.server=Local\njenkins.reviewed=false',
                    pushDescription: 'Pushed from Jenkins',
                    pushIncremental: false
                ]
            ]
        ])
   }
  
}

