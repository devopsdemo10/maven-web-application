node

{
def mavenhome = tool name: "maven-3.8.6"
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])

echo "the node name is: ${env.NODE_NAME}"
echo "the build number is ${env.BUILD_NUMBER}"
echo "the job name is ${env.JOB_NAME}"
echo "the workspace path is ${env.WORKSPACE}"

stage('checkoutCode')
{
git branch: 'development', credentialsId: '2ca26487-f4d1-4171-a75d-ae3b6892c2f6', url: 'https://github.com/devopsdemo10/maven-web-application.git'
}
stage('build')
{
sh "${mavenhome}/bin/mvn clean package"
}
stage('sonarqubereport')
{
sh "${mavenhome}/bin/mvn sonar:sonar"
}
stage('uploadnexus')
{
sh "${mavenhome}/bin/mvn deploy"
}
stage('deploytotomcat')
{
sshagent(['9b9de740-0002-433d-84fa-95bfe6e60d39']) {
sh "scp -o StrictHostKeyChecking=no target/*.war   ec2-user@172.31.43.121:/opt/apache-tomcat-9.0.64/webapps/"
}
}
}
