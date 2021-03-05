node
{
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([pollSCM('H * * * * ')])])
    def MavenHome = tool name :"maven3.6.3"
    stage('checkout')
    {
        git branch: 'development', credentialsId: 'ce9e5b27-5515-4c29-8a54-a7c0b59ed2f6', url: 'https://github.com/vishnuu93/maven-web-application.git'
    }
    
    stage('build')
    {
        sh "${MavenHome}/bin/mvn clean package"
    }
    stage('Executing Sonar Qube report')
    {
        sh "${MavenHome}/bin/mvn sonar:sonar"
    }
    stage('Storing artifacts to nexus')
    {
        sh "${MavenHome}/bin/mvn deploy"
    }
    stage('deployapp to tomcat')
    {
        sshagent(['8b470bfe-1416-47a7-89e5-35ad8430e7d6']) {

            sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.232.196.81://opt/apache-tomcat-9.0.43/webapps/"
        }
    }
}
