#!groovy

properties([
    buildDiscarder(logRotator(numToKeepStr: '1')),
    pipelineTriggers([
        pollSCM('* * * * *')
    ])
])

node{
   def maven_homepath=tool name:'Maven',type:"maven"
   stage('GIT-CheckOutCode')
   {
    git credentialsId: '8b1af5dc-180d-449d-98a8-762961c4492b', url: 'https://github.com/harishgb/maven-web-application.git'  
    }
   stage('Maven-Build code')
   {
       sh "${maven_homepath}/bin/mvn clean package"
   }
    stage('SQ-Build code')
   {
       sh "${maven_homepath}/bin/mvn sonar:sonar"
   }
   stage('Nexus-Build code')
   {
       sh "${maven_homepath}/bin/mvn deploy"
   }
   stage('Tomcat-Build code')
   {
       sh "cp $WORKSPACE/target/*.war /opt/apache-tomcat-9.0.16/webapps"
   }
}
