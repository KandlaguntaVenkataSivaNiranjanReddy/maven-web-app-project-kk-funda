properties([pipelineTriggers([pollSCM('* * * * *')])])

node 
{       
    def mavenHome = tool name: 'maven'

    stage('Stage 1: Checkout')
    {
        git branch: 'development', credentialsId: '8487cd1c-004d-4db2-9ef5-9386d3641418', url: 'https://github.com/KandlaguntaVenkataSivaNiranjanReddy/maven-web-app-project-kk-funda.git'
    }

    stage('Stage 2: Build')
    {
        sh "${mavenHome}/bin/mvn clean package"
    }

    stage('Stage 3: SonarQube Report')
    {
        sh "${mavenHome}/bin/mvn sonar:sonar"
    }
    stage('Stage 4: arifact to nexus')
   {
        sh "${mavenHome}/bin/mvn clean package sonar:sonar deploy"
   }    
    stage('Stage 5: Deploy to tomcat')
   {
    sshagent(['8cde9fd2-eccc-4abf-8304-260130705f45']) 
   {
     sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.49.65.133:/opt/apache-tomcat-9.0.91/webapps/"
    }    
   }    
}

