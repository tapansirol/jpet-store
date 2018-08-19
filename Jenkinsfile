node{
  stage ('cloning the repository'){
      git 'https://github.com/tapansirol/jpet-store'
  }
  stage ('Build') {
      withMaven(jdk: 'JDK_local', maven: 'MVN_Local') {
      bat 'mvn clean package'
    }
  }
stage ('Push to UCD...') {
       step([$class: 'UCDeployPublisher',
            siteName: 'ucd-server',
            component: [
                $class: 'com.urbancode.jenkins.plugins.ucdeploy.VersionHelper$VersionBlock',
                componentName: 'UCD - Pipeline',
                createComponent: [
                    $class: 'com.urbancode.jenkins.plugins.ucdeploy.ComponentHelper$CreateComponentBlock'
                ],
                delivery: [
                    $class: 'com.urbancode.jenkins.plugins.ucdeploy.DeliveryHelper$Push',
                    pushVersion: '${BUILD_NUMBER}',
                    baseDir: '\\workspace\\jpet-store-test-tapan\\target',
                    fileIncludePatterns: '*.war',
                    fileExcludePatterns: '',
                    pushProperties: 'jenkins.server=Local\njenkins.reviewed=false',
                    pushDescription: 'Pushed from Jenkins',
                    pushIncremental: false
                ]
            ]
        ])
   }
  
}

