node{
  
try{

def mavenHome = tool name: 'maven3.8.5'

echo "The Job name is: ${env.JOB_NAME}"
echo "The Build number is: ${env.BUILD_NUMBER}"
echo "The node name is: ${env.NODE_NAME}"

  properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])

//Checkout Code State
stage('CheckoutCode'){
sendSlackNotifications("STARTED")
git branch: 'development', credentialsId: '2b4cd540-c650-491b-90ce-1fb6a82a2972', url: 
'https://github.com/Citybank-applications/maven-web-application.git'
}

//Build
stage('Build'){
sh "${mavenHome}/bin/mvn clean package"
}

  /*
//Execute SonarQube Report
stage('ExecuteSonarQubeReport'){
sh "${mavenHome}/bin/mvn sonar:sonar"
}

//uploadArtifacts Into Nexus
stage('UploadArtifactsIntoNexus'){
sh "${mavenHome}/bin/mvn deploy"
}


//Deploy App into Tomcat Server
stage('DeployApp')
sshagent(['7b19e750-aecf-4016-926e-417645461938']) {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.235.243.201:/opt/apache-tomcat-9.0.68/webapps"
}
}
*/

}//try closing
catch(e){
currentBuild.result= "FAILURE"
}
finally{
sendSlackNotifications(currentBuild.result)
}
  
}//node closing

//Function for Slack Notification

def sendSlackNotifications(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus =  buildStatus ?: 'SUCCESS'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESS') {
    color = 'GREEN'
    colorCode = '#00FF00'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend (color: colorCode, message: summary)
}




