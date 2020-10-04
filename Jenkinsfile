node
{
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])])
    echo "Build number is  ${env.BUILD_NUMBER}"
    def mavenHome=tool name:"maven-3.6.3"
    stage('checkoutcode')
    {
        git branch: 'development', credentialsId: 'a8a45ce2-8f08-49cd-a15c-0ce30d9c7d61', url: 'https://github.com/jithu0446/maven-web-application.git'
    }
    stage('Build')
    {
        sh "${mavenHome}/bin/mvn clean package"
    }
    stage('SonarQube report')
    {
        sh "${mavenHome}/bin/mvn sonar:sonar"
    }
    stage('UploadingArtifactsinto Nexus')
    {
        sh "${mavenHome}/bin/mvn deploy"
    }
    stage('DeploytoTomcat')
    {
        sshagent(['TomcatServerCredentials']) {
        sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@52.66.236.214:/opt/apache-tomcat-9.0.38/webapps/"
        }
    
    }
    stage('Email notification')
    {
        mail bcc: 'jithu0446@gmail.com', body: '''Build is over bro

Regards
Jithu''', cc: 'jithu0446@gmail.com', from: '', replyTo: '', subject: "Build is over.. ${env.BUILD_NUMBER}", to: 'jithu0446@gmail.com'
    }
    
}
