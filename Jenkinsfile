node
{
def mavenHome = tool name: "maven3.8.5"
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '5', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])]) 
echo "the job name is: ${env.JOB_NAME}"  
stage('code'){
git branch: 'development', credentialsId: 'f6282c52-f5e2-40a4-a24b-62e00fe05253', url: 'https://github.com/mithunsoftware/maven-web-application.git'
}

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
}
