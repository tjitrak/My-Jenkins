node {
    
    def mvn = tool (name: 'M2_HOME', type: 'maven') + '/bin/mvn'
    
    stage('SCM Checkout') {
        checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/tjitrak/webapp_maven_deploy.git']]])
    }
    stage('SonarQube scan Quality Code') {  
    }
    stage('Checkmarx scan Security Code') {
    }
    stage('IndependencyCheck Scan') {
    }
    stage('Build') {
        sh "${mvn} clean install package"
    }
}
