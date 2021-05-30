pipeline
{
    //agent any
    agent
    {
        label 'wallmart-node1'
    }
    tools
    {
        maven 'maven3.8.1'
    }
    triggers
    {
        //PollSCM
        pollSCM('* * * * *')
        //Build Periodically
        //cron('* * * * *')
        //GitHub WebHook
        //githubPush()
    }
    options
    {
        buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '3', daysToKeepStr: '', numToKeepStr: '3'))
    
        timestamps()
    }
    stages
    {
        stage('CheckoutCode')
        {
            steps
            {
                git branch: 'development', credentialsId: '3a37c74a-1176-422d-97c6-25d6e1ae50ef', url: 'https://github.com/senthilar257/maven-web-application.git'
            }
        }
        stage('Build')
        {
            steps
            {
                sh "mvn clean package"
            }
        }
        stage('ExecuteSonarQubeReport')
        {
            steps
            {
                sh "mvn clean sonar:sonar"
            }
        }
        stage('UploadArtifactsintoNexus')
        {
            steps
            {
                sh "mvn clean deploy"
            }
        }
        stage('DeployAppintoTomcatServer')
        {
            steps
            {
                sshagent(['c394635b-2d9a-4906-bf8d-ddfb0021b370']) {
                sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@65.1.100.255:4565:/opt/apache-tomcat-9.0.45/webapps/"
            }
        }
        
    }
}
post
{
    success
    {
     emailext body: '''Hi Senthil,
     Build is over.Declarative way - Success
     Regards,
     Senthil A
     9361226675''', subject: 'Build Over.Declarative way - Success', to: 'sentilg1g@gmail.com'
    }
    failure
    {
     emailext body: '''Hi Senthil,
     Build is over.Declarative way - Failed
     Regards,
     Senthil A
     9361226675''', subject: 'Build Over.Declarative way - Failed', to: 'sentilg1g@gmail.com'   
    }
}
}
