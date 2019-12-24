node {
    stage('Checkout Code') {
    checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/tjitrak/webapp_maven_deploy.git']]])
    }
    stage('Checkmarx Scan') {  
    }
    stage('SonarQube Scan') {
    }
    stage('IndependencyCheck Scan') {
    }
    stage('Build') {
    }
}
