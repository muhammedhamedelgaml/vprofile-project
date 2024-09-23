pipeline{
  agent any 
  tools {
    maven  "maven3"
    jdk  "jdk11"
   } 

   enviroment{
    SNAP_REPO = 'vprofile-snapshot'
    NEXUS_USER = 'admin'
    NEXUS_PASS = 'admin123'
    RELEASE_REPO = 'vprofile-release'
    CENTRAL_REPO = 'vpro-maven-central' 
    NEXUS_GRP_REPO = 'vpro-maven-group'
    NEXUSIP = 'localhost'
    NEXUSPORT =  '8081'
    NEXUS_LOGIN = 'nexuslogin' 
    }

  stages{
      stage('Build')
       steps{ 
         sh 'mvn -s settings.xml -DskipTests install'
       }
  }
  
}
