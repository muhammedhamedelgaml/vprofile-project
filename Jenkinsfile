def COLOR_MAP = [
  'SUCCESS' : 'good',
  'FAILURE' : 'danger'
]
pipeline{
  agent any 
  tools {
    maven  "maven3"
    jdk  "jdk11"
   } 
   environment{
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
      stage('Build'){
       steps{ 
         sh 'mvn -s settings.xml -DskipTests install'
       }
       post {
              success {
                echo 'Now Archiving...'
                archiveArtifacts artifacts: '**/target/*.war'
                }
            }
      } 
  stage("uploadArtifact"){
   steps{
     nexusArtifactUploader(
        nexusVersion: 'nexus3',
        protocol: 'http',
        nexusUrl: "${NEXUSIP}:${NEXUSPORT}",
        groupId: 'QA',
        version: "${env.BUILD_ID}-${env.BUILD_TIMESTAMP}",
        repository: "${RELEASE_REPO}",
        credentialsId: "${NEXUS_LOGIN}",
        artifacts: [
           [ 
              artifactId: 'vproapp' ,
              classifier: '',
              file: 'target/vprofile-v2.war',
              type: 'war'
           ] 

        ]

     )
     
   }
 }
  }
  
  post {
        always  {
            echo 'slack notification.'
            slackSend channel: '#cicdjenkins',
            color:  COLOR_MAP[currentBuild.currentResult],
            message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER} \n for more info visit : ${env.BUILD_URL} " 
        }
  }

}
