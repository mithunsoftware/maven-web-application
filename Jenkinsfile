node
{
def mavenHome = tool name: "maven3.8.5"
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '5', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])]) 
echo "the job name is: ${env.JOB_NAME}"  
stage('code'){
git branch: 'development', credentialsId: 'f6282c52-f5e2-40a4-a24b-62e00fe05253', url: 'https://github.com/mithunsoftware/maven-web-application.git'
}
 try{
stage('Build'){
sh "${mavenHome}/bin/mvn clean package"
}

stage('sonar'){
sh "${mavenHome}/bin/mvn sonar:sonar"
}
/*
stage('nexus'){
sh "${mavenHome}/bin/mvn deploy"    
}

stage('tomcat'){
sshagent(['8108e76c-2b26-45ed-a387-0369ee8eb28d']) {
sh "scp -o  StrictHostKeyChecking=no target/maven-web-application.war ec2-user@3.109.48.17:/opt/apache-tomcat-9.0.64/webapps/"
 
}
    
    
}
*/

 catch (e){
 currentbuild.result = "FAILED"
 throw e 
 } 
 finally{
 slacknotification(currentBuild.result)
 }
  
 }//node closing
def notifyBuild(String buildStatus = 'STARTED') {
  
  buildStatus =  buildStatus ?: 'SUCCESSFUL'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"                             
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESSFUL') {
    color = 'GREEN'
    colorCode = '#00FF00'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend (color: colorCode, message: summary)
}
