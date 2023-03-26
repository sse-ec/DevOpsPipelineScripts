node
{
     echo "GitHub BranchName ${env.BRANCH_NAME}"
     echo "Jenkins Job Number ${env.BUILD_NUMBER}"
     echo "Jenkins Node Name ${env.NODE_NAME}"
     echo "Jenkins Home ${env.JENKINS_HOME}"
     echo "Jenkins Url ${env.JENKINS_URL}"

   properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])])
    def mavenHome = tool name: "Maven3.9.0"
    
    stage("CheckOutCode")
    {
        git credentialsId: '88edce31-1efa-4545-8fab-ae5d311f03cd', url: 'https://github.com/sse-ec/maven-web-application.git'
    }
    stage("Build")
    {
    bat "${mavenHome}/bin/mvn clean package"
    }
    stage("ExecuteSonarQubeReport")
    {
      bat "${mavenHome}/bin/mvn clean sonar:sonar"  
    }
     stage("UploadArtifactIntoNexus")
    {
      bat "${mavenHome}/bin/mvn clean deploy"  
    }
    stage("DeletingWarFile")
    {
     bat("del C:\\Softwares\\apache-tomcat-9\\apache-tomcat-9.0.73\\webapps\\maven-web-application.war")   
    }
    stage("DeployApplicationIntoTomcatServer")
    {
    bat("xcopy C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\Pipeline-project\\target\\maven-web-application.war C:\\Softwares\\apache-tomcat-9\\apache-tomcat-9.0.73\\webapps")
    }
    stage("SendEmailNotification")
    {
     emailext body: '''Build is over....


    Thanks,
    Chinna.''', subject: 'Build Is Over ....', to: 'chinna.cpv@gmail.com'   
    }
}
