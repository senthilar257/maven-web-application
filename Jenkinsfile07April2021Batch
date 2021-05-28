node
{
    def mavenHome = tool name: "maven3.8.1"
    
    //properties([[$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
    
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
    
    stage('CheckoutCode')
    {
        git branch: 'development', credentialsId: '3a37c74a-1176-422d-97c6-25d6e1ae50ef', url: 'https://github.com/senthilar257/maven-web-application.git'
    }
    
    stage('Build')
    {
        sh "${mavenHome}/bin/mvn clean package"
    }
  /*  
    stage('ExecuteSonarQubeReport')
    {
        sh "${mavenHome}/bin/mvn sonar:sonar"
    }
    
    stage('UploadArtifactsintoNexus')
    {
        sh "${mavenHome}/bin/mvn deploy"
    }
    
    stage('DeployAppintoTomcatServer')
    {
        sshagent(['c394635b-2d9a-4906-bf8d-ddfb0021b370']) {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@15.207.116.19:/opt/apache-tomcat-9.0.45/webapps/ "
}  
    } */
    stage('SendEmailNotification')
    {
      emailext body: '''Hi Senthil,
     Build is over.
     Regards,
     Senthil A
     9361226675''', subject: 'Build Over..', to: 'sentilg1g@gmail.com'
    }
    
}
