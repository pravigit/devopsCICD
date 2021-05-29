pipeline { 
agent any
tools {
maven 'MAVEN_HOME'
}
stages{
 stage('Get GIT source for Build Deployment')
 {
   steps 
   {git credentialsId: 'PraveenGIT', url: 'https://github.com/pravigit/devopsCICD.git'}
 }
 stage('Test')
 {
    steps {sh 'mvn test'}
 }
 stage('Package')
 {
     steps {sh 'mvn package'}
 } 
 

stage('Upload to Nexus')
{
steps { 
nexusArtifactUploader artifacts: [[artifactId: 'webapp', classifier: '', file: 'webapp/target/webapp.war', type: '1.0']], credentialsId: 'NexusCredentials', groupId: 'com.demo', nexusUrl: '10.245.128.230:8081/', nexusVersion: 'nexus3', protocol: 'http', repository: 'Jenkins-ci-maven-nexus-repo', version: '6.0'
}
}

stage('Download from Nexus')
{
steps
{
sh 'wget --user=admin --password=admin123  http://10.245.128.230:8081/repository/Jenkins-ci-maven-nexus-repo/com/demo/webapp/6.0/webapp-6.0.1.0.war'
sh 'mv webapp-6.0.1.0.war devops.war'
}
}
  stage('Deploy')
  {
     steps { deploy adapters: [tomcat8(credentialsId: 'TomcatCredentails', path: '', url: 'http://10.245.128.230:8090/')], contextPath: null, war: '**/*.war'}
  } 
  
}
}
