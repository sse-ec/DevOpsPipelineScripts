node
{
 echo "GitHub BranchName ${env.BRANCH_NAME}"
 echo "Jenkins Job Number ${env.BUILD_NUMBER}"
 echo "Jenkins Node Name ${env.NODE_NAME}"
 echo "Jenkins Home ${env.JENKINS_HOME}"
 echo "Jenkins Url ${env.JENKINS_URL}"
 
 
 properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '2', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])])
 
 def mavenHome = tool name: "maven3.9.0"
 
stage("GitHub")
{
 git credentialsId: '2966c7ef-17b5-47b5-ae3d-d7c82f1d716e', url: 'https://github.com/sse-ec/maven-web-application.git'   
}
stage("Build")
{
  bat "${mavenHome}/bin/mvn clean package"  
}
/*stage("SonarQubeReoprts")
{
  bat "${mavenHome}/bin/mvn sonar:sonar"  
} */
stage("UploadArtifactsintoNexus")
{
  bat "${mavenHome}/bin/mvn deploy"  
}
stage("BuildemailNotification")
mail bcc: '', body: '''echo "GitHub BranchName ${env.BRANCH_NAME}"
 echo "Jenkins Job Number ${env.BUILD_NUMBER}"
 echo "Jenkins Node Name ${env.NODE_NAME}"
 echo "Jenkins Home ${env.JENKINS_HOME}"
 echo "Jenkins Url ${env.JENKINS_URL}"

Thanks,
Chinnappa.''', cc: '', from: '', replyTo: '', subject: 'Build Success', to: 'chinna.cpv@yahoo.com'

}
