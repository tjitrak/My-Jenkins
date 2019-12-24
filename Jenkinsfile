node {
    
    def sonarUrl = 'sonar.host.url=http://sonar.1secure.com:9000'
    def mvn = tool (name: 'M2_HOME', type: 'maven') + '/bin/mvn'
    
    stage('SCM Checkout') {
        checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/tjitrak/webapp_maven_deploy.git']]])
    }
    stage('SonarQube scan Quality Code') {  
        withCredentials([string(credentialsId: 'sonarqube', variable: 'jenkins')]) {
        def sonarToken = "sonar.login=${sonarToken}"
        sh "${mvn} sonar:sonar -D${sonarUrl}  -D${sonarToken}"
	 }
        
    }
    stage('Checkmarx scan Security Code') {
    }
    stage('IndependencyCheck Scan') {
    }
    stage('Build') {
        sh "${mvn} clean install package"
    }
}
