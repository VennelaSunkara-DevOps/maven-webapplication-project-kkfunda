node
{
      // /var/lib/jenkins/tools/hudson.tasks.Maven_MavenInstallation/maven-3.9.6/bin
    node
{
	echo "git branch name: ${env.BRANCH_NAME}"
	echo "build number is: ${env.BUILD_NUMBER}"
	echo "node name is: ${env.NODE-NAME}"

	def mavenHome =tool name:  "maven-3.9.6"
	try
	{
       stage('git checkout')
     {
           git branch: 'development', credentialsId: '95fa1633-e702-4925-b440-3aaf8e47c3ac', url:          'https://github.com/VennelaSunkara-DevOps/maven-webapplication-project-kkfunda.git'
     }
     stage('Maven Build')
     {
           sh "${mavenHome}/bin/mvn clean package"
     }
      stage('SQ Report')
     {
              sh "${mavenHome}/bin/mvn sonar:sonar"
     }
     stage('Deploy into Nexus')
     {
              sh "${mavenHome}/bin/mvn deploy"
     }
     stage('Deploy to Tomcat') 
    {
      
      sh """
         curl -u sai:Venni@123 \
         --upload-file /var/lib/jenkins/workspace/jio-dev-scriptedway-PL/target/maven-web-application.war \
         "http://13.201.18.101:8080/manager/text/deploy?path=/maven-web-application&update=true"
          
        """
    }
	} // try ending
	catch (e) {
		  currentBuild.result = "FAILED"
	} finally {
	  notifyBuild(currentBuild.result)
	}
} //node ending
def notifyBuild(String buildStatus = "STARTED") {
	buildStatus = buildStatus ?: 'SUCCESS'
	def colorName = 'RED'
	def colorCode = '#FF0000'
	def subject = "${buildStatus}: job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
	def summary = "${subject} (${env.BUILD_URL})"

	if (buildStatus == 'STARTED') {
		color = 'YELLOW'
		colorCode = '#FFFF00'
	}else if (buildStatus == 'SUCCESS') {
		color = 'GREEN'
		colorCode = '#00FF00'
	} else {
		color = 'RED'
		colorCode = '#FF0000'
	}
	slackSend (color: ColorCode, message: summary, channel: '#jio-dev')
	slackSend (color: ColorCode, message: summary, channel: '#jio-devops')

}
}
