node {
    echo "git branch name: ${env.BRANCH_NAME}"
    echo "build number is: ${env.BUILD_NUMBER}"
    echo "node name is: ${env.NODE_NAME}"

    def mavenHome = tool name: "maven-3.9.6"

    try {
        stage('Git Checkout') {
            git branch: 'development', credentialsId: '95fa1633-e702-4925-b440-3aaf8e47c3ac', url: 'https://github.com/VennelaSunkara-DevOps/maven-webapplication-project-kkfunda.git'
        }

        stage('Maven Build') {
            sh "${mavenHome}/bin/mvn clean package"
        }

        stage('SonarQube Report') {
            // Optional: use withSonarQubeEnv() if configured in Jenkins
            sh "${mavenHome}/bin/mvn sonar:sonar"
        }

        stage('Deploy into Nexus') {
            sh "${mavenHome}/bin/mvn deploy"
        }

        stage('Deploy to Tomcat') {
            sh '''
                curl -u sai:Venni@123 \
                --upload-file target/maven-web-application.war \
                "http://13.201.18.101:8080/manager/text/deploy?path=/maven-web-application&update=true"
            '''
        }

    } catch (e) {
        currentBuild.result = "FAILURE"
        echo "❌ Pipeline failed: ${e}"
        throw e
    } finally {
        notifyBuild(currentBuild.result)
    }
}

def notifyBuild(String buildStatus = "STARTED") {
    buildStatus = buildStatus ?: 'SUCCESS'
    def colorName = 'RED'
    def colorCode = '#FF0000'
    def subject = "${buildStatus}: job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
    def summary = "${subject} (${env.BUILD_URL})"

    if (buildStatus == 'STARTED') {
        colorName = 'YELLOW'
        colorCode = '#FFFF00'
    } else if (buildStatus == 'SUCCESS') {
        colorName = 'GREEN'
        colorCode = '#00FF00'
    } else {
        colorName = 'RED'
        colorCode = '#FF0000'
    }

    // ✅ Fix: Correct variable name and case
    slackSend(color: colorCode, message: summary, channel: '#jio-dev')
    slackSend(color: colorCode, message: summary, channel: '#jio-devops')
}
