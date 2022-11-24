node{
    
    def mavenHome = '/var/lib/jenkins/tools/hudson.tasks.Maven_MavenInstallation/maven_3.8.5'
   
   echo "The job name is : ${env.JOB_NAME}"
   echo "The build num is : ${env.BUILD_NUMBER}"
   echo "The node name is : ${env.NODE_NAME}"
   
   properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])]) 

//checkoutcode stage
stage('CheckooutCode'){
git branch: 'development', credentialsId: '638fcf9e-f596-41f3-9ae2-13bbdccaf292', url: 'https://github.com/devopsgayathri/maven-web-application.git'
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

//UploadArtifacts into Nexus
stage('UploadArtifactsintoNexus'){
sh "${mavenHome}/bin/mvn deploy"
}

//Deploy App into Tomcat server
stage('DeployApp'){
sshagent(['de624ee1-742e-4970-8ccd-4d7f9f8cfe4a']) {
  sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.47.214:/opt/apache-tomcat-9.0.68/webapps"  
}
}

*/
  
}//node closing

