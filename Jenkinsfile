Node{
	stage(’SCM Checkout’){
		git ‘https://github.com/tjitrak/webapp_maven_deploy.git’
	}

	stage(‘Build Package’){
	sh ‘mvn package’
	}	
}
