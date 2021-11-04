node{
    def mavenHome=tool name:"maven 3.6.2"
    stage('Checkout')
    {
        git branch: 'development', credentialsId: '0a0557d9-5a09-4b74-83f4-3522c5837267', url: 'https://github.com/GitHub-SuryaL/maven-web-application.git'
    }
    stage('Build')
    {
        sh "${mavenHome}/bin/mvn clean package"
        
    }
    stage('SonarQubeReport')
    {
        sh "${mavenHome}/bin/mvn sonar:sonar"
    }
    
    stage('UploadingArtifactsintoNexus')
    {
        sh "${mavenHome}/bin/mvn clean deploy"
    }
    
    stage('DeployAppIntoTomcat')
    {
        sshagent(['ec85dc8a-71bf-4260-b6a7-30a416a25cf7']) {
    // some block
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@65.0.199.44:/opt/apache-tomcat-9.0.54/webapps/"
    
    }
    }
    stage('EmailNotification')
    {
        
        
        emailext body: '''Hi,
Build is over, Please check it.

Regards
Surya''', subject: 'Build Status', to: 'hisurya3@gmail.com'

    }
    

}
