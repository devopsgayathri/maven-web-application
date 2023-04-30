node('nodes'){
    
    try{
    
    def mavenHome = '/home/ec2-user/node1/tools/hudson.tasks.Maven_MavenInstallation/maven_3.8.5'
   
   echo "The job name is : ${env.JOB_NAME}"
   echo "The build num is : ${env.BUILD_NUMBER}"
   echo "The node name is : ${env.NODE_NAME}"
   
   properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])]) 

//checkoutcode stage
stage('CheckooutCode'){
    sendSlackNotifications("STARTED")
    
git branch: 'development', credentialsId: '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', url: 'https://github.com/devopsgayathri/maven-web-application.git'
}

//Build
stage('Build'){
sh "${mavenHome}/bin/mvn clean package"
}


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
sshagent(['wPJKgmUC1eVYPIbM+h6nEfdlVGHQ0G1WxOFR4k4LEbM']) {
  sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.47.214:/opt/apache-tomcat-10.1.8/webapps"  
}
}


  
} //try closing
    
    catch(e){
currentBuild.result = "FAILURE"
}
finally{
sendSlackNotifications(currentBuild.result)
}
}//node closing

//Function for slack notification

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

