node
{

def mavenHome = tool name: "maven3.6.3"	

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '3', daysToKeepStr: '', numToKeepStr: '3')), pipelineTriggers([pollSCM('* * * * *')])])

stage ('Get the Code from GitHub')
{
git branch: 'development', credentialsId: '74040a08-fe92-4f3f-a428-606ab50316a2', url: 'https://github.com/panchalsantosh8286/maven-web-application.git'
}

stage ('Build the package')
{
sh "${mavenHome}/bin/mvn clean package"
}

stage ('Executing the SonarQube Report')
{ 
sh "${mavenHome}/bin/mvn sonar:sonar"
}

stage ('Upload artifact/package into nexux repository')
{ 
sh "${mavenHome}/bin/mvn deploy"
}

stage ('Deploy application into Tomcat server')
{
sshagent(['bc6b2f1d-8ecb-48aa-bc74-3a02ba4bf9cc']) {
 sh 'scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@15.206.116.153:/opt/apache-tomcat-9.0.46/webapps/'
}
}

stage ('Send an email notification')
{
emailext body: '''Build Over....

Thanks
Santosh Panchal''', subject: 'Build Over.....', to: 'panchal.vivaan2015@gmail.com'
}

}
