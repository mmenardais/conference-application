// Powered by Infostretch

timestamps {

node () {

	stage ('conference-application - Checkout') {
 	 checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', url: 'https://github.com/mmenardais/conference-application.git']]])
	}
	stage ('conference-application - Build') {
 			// Maven build step
	withMaven(maven: 'maven 3.6') {
 			if(isUnix()) {
 				sh "mvn clean package "
			} else {
 				bat "mvn clean package "
			}
 		}
		archiveArtifacts allowEmptyArchive: false, artifacts: '**/target/*.war', caseSensitive: true, defaultExcludes: true, fingerprint: false, onlyIfSuccessful: false
	}
}
}