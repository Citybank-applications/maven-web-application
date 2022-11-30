node{

def mavenHome = tool name: 'maven3.8.5'

echo "The Job name is: ${env.JOB_NAME}"
echo "The Build number is: ${env.BUILD_NUMBER}"
echo "The node name is: ${env.NODE_NAME}"

  properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])

//Checkout Code State
stage('CheckoutCode'){
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

}//node closing

