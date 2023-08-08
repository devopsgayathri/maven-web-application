node('nodes'){
    
    try{
    
def mavenHome = tool name: 'maven 3.9.3', type: 'maven'
   
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
    }
}

        

