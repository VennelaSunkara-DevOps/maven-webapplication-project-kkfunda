node
{
      // /var/lib/jenkins/tools/hudson.tasks.Maven_MavenInstallation/maven-3.9.6/bin
      def mavenHome =tool name:  "maven-3.9.6"
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
         "http://52.66.126.66:8080/manager/text/deploy?path=/maven-web-application&update=true"
          
        """
    }
}
