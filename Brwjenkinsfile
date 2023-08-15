// SCRIPTED PIPELINE SCRIPT 
node{

def MavenHome = tool name: "maven3.9.3" // if maven is configured in global tool configuration 

// code checkout From Git
stage ('CheckoutCode'){
checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'f2d3d2ce-6cbf-4d0b-908d-de854e57183d', url: 'https://github.com/MydevopsCourse/maven-web-application--main.git']])   
}

// Building the Job 
stage ('Build'){
 sh "$MavenHome/bin/mvn clean package"  
}

// Code Quality Checking 
stage ('SonanQube Report'){
  sh "$MavenHome/bin/mvn sonar:sonar"  
}

// Upload Artifact into artifact Repo
stage ('Deploying into nexus'){
  sh "$MavenHome/bin/mvn deploy"  
}
// Deploying application to the tomcat server
stage ('Deploying War File'){
sshagent(['5acd34a2-0abb-494b-a1a9-be08120fbe5a']) {
   sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ubuntu@172.31.0.114:/opt/tomcat/webapps"
    }  
  }

} // pipeline closing
