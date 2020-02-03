node {

    def mvn = tool (name: 'M2_HOME', type: 'maven') + '/bin/mvn'	
    
    stage('GitLab Checkout') {
	deleteDir()   
        checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/tjitrak/webapp_maven_deploy.git']]])
    
    }
    stage('SonarQube scan Quality Code') {  
	withSonarQubeEnv('Java-Scan') { 
          sh "${mvn} sonar:sonar"
        }
        
    }
//	
//   stage('Checkmarx scan Security Code') {
//	step([$class: 'CxScanBuilder', comment: '', credentialsId: '', enableProjectPolicyEnforcement: true, excludeFolders: '', excludeOpenSourceFolders: '', exclusionsSetting: 'global', failBuildOnNewResults: false, failBuildOnNewSeverity: 'HIGH', filterPattern: '''!**/_cvs/**/*, !**/.svn/**/*,   !**/.hg/**/*,   !**/.git/**/*,  !**/.bzr/**/*, !**/bin/**/*,
// !**/obj/**/*,  !**/backup/**/*, !**/.idea/**/*, !**/*.DS_Store, !**/*.ipr,     !**/*.iws,
// !**/*.bak,     !**/*.tmp,       !**/*.aac,      !**/*.aif,      !**/*.iff,     !**/*.m3u, !**/*.mid, !**/*.mp3,
// !**/*.mpa,     !**/*.ra,        !**/*.wav,      !**/*.wma,      !**/*.3g2,     !**/*.3gp, !**/*.asf, !**/*.asx,
// !**/*.avi,     !**/*.flv,       !**/*.mov,      !**/*.mp4,      !**/*.mpg,     !**/*.rm,  !**/*.swf, !**/*.vob,
// !**/*.wmv,     !**/*.bmp,       !**/*.gif,      !**/*.jpg,      !**/*.png,     !**/*.psd, !**/*.tif, !**/*.swf,
// !**/*.jar,     !**/*.zip,       !**/*.rar,      !**/*.exe,      !**/*.dll,     !**/*.pdb, !**/*.7z,  !**/*.gz,
// !**/*.tar.gz,  !**/*.tar,       !**/*.gz,       !**/*.ahtm,     !**/*.ahtml,   !**/*.fhtml, !**/*.hdm,
// !**/*.hdml,    !**/*.hsql,      !**/*.ht,       !**/*.hta,      !**/*.htc,     !**/*.htd, !**/*.war, !**/*.ear,
// !**/*.htmls,   !**/*.ihtml,     !**/*.mht,      !**/*.mhtm,     !**/*.mhtml,   !**/*.ssi, !**/*.stm,
// !**/*.stml,    !**/*.ttml,      !**/*.txn,      !**/*.xhtm,     !**/*.xhtml,   !**/*.class, !**/*.iml, !Checkmarx/Reports/*.*''', fullScanCycle: 10, generatePdfReport: true, groupId: '00000000-1111-1111-b111-989c9070eb11', includeOpenSourceFolders: '', incremental: true, osaArchiveIncludePatterns: '*.zip, *.war, *.ear, *.tgz', osaEnabled: true, osaInstallBeforeScan: false, password: '{AQAAABAAAAAQEvdTgdooU71WECpu1PLPs8d0NPtraCvMwqDc2BlMGIs=}', preset: '36', projectName: 'Build_Project_by_Pipeline', sastEnabled: true, serverUrl: 'http://techcrub.thddns.net:2440', sourceEncoding: '1', username: '', vulnerabilityThresholdResult: 'FAILURE', waitForResultsEnabled: true])
//    }

    stage('Maven Build') {
        sh "${mvn} clean install package" 
    }    
	
	stage('Jfrog archive Artifacts') {
		archiveArtifacts artifacts: 'multi3/target/*.war', onlyIfSuccessful: true
	}
	
	stage('Build Docker Image') {
		sshPublisher(publishers: [sshPublisherDesc(configName: 'docker-host', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '.', remoteDirectorySDF: false, removePrefix: '/multi3/target', sourceFiles: 'multi3/target/*.war')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])	
	}
	
	stage('Deploy On Dev'){
		//sshagent(['dockeradmin']) {
		sshagent (credentials: ['dockeradmin']){
    			
			def containerStop = "docker stop devops-demo-container"
			def containerRemove = "docker rm devops-demo-container"
			def dockerRemove = "docker rmi devops-demo tomcat"
			def dockerBuild = "docker build -t devops-demo ."
			def dockerRun = "docker run -d -p 8080:8080 --name devops-demo-container devops-demo"
			def Remove = "rm -f devops-demo.war"
			def Rename = "mv multi3-3.9-TETRA.war devops-demo.war"
			sh "ssh -o StrictHostKeyChecking=no tjitrak@192.168.1.121 ${Remove}"
			sh "ssh -o StrictHostKeyChecking=no tjitrak@192.168.1.121 ${Rename}"
			sh "ssh -o StrictHostKeyChecking=no tjitrak@192.168.1.121 ${containerStop}"
			sh "ssh -o StrictHostKeyChecking=no tjitrak@192.168.1.121 ${containerRemove}"
			sh "ssh -o StrictHostKeyChecking=no tjitrak@192.168.1.121 ${dockerRemove}"
			sh "ssh -o StrictHostKeyChecking=no tjitrak@192.168.1.121 ${dockerBuild}"
			sh "ssh -o StrictHostKeyChecking=no tjitrak@192.168.1.121 ${dockerRun}"
		}
	}
}
