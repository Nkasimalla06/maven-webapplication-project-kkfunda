




pipeline
{
	
   agent any
   tools
   {
      maven  "maven"
   }
   
   stages
   {
           stage('git checkout')
           {
              steps
              {
                 notifyBuild('STARTED')
                 git branch: 'TEST', url: 'https://github.com/Nkasimalla06/maven-webapplication-project-kkfunda.git'
              }
		   }
              stage ('compile')
              {
                  steps
                  {
                   sh "mvn compile"
                  }
              }
              stage ('build')
              {
                  steps 
                  {
                      sh "mvn clean package"
                  }
              }
              stage ('SQ Report')
              {
                  steps
                  {
                      sh "mvn sonar:sonar"
                  }
              }
              stage ('Deploy to Nexus')
              {
                  steps
                  {
                      sh "mvn clean deploy"
                  }
              }
              stage('Deploy to tomcat')
           {
              steps
              {
                 sh """

      curl -u navin:9700172096@Nn \
--upload-file /var/lib/jenkins/workspace/airtel-deploy/target/maven-web-application.war \
"http://43.204.234.47:8080/manager/text/deploy?path=/maven-web-application&update=true"
          
        """
              }
           }
           
          

     } //stages ending
post {
  success {

    script
    {
     notifyBuild(currentBuild.result)
    }
    
  }
  failure {

  script
  {
    notifyBuild(currentBuild.result)

  }
   
  }
}


} //pipeline ending

// Notification method
def notifyBuild(String buildStatus = 'STARTED') {
    buildStatus = buildStatus ?: 'SUCCESS'

    def colorCode
    def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
    def summary = "${subject} (${env.BUILD_URL})"

    switch (buildStatus) {
        case 'STARTED':
            colorCode = '#FFFF00' // Yellow
            break
        case 'SUCCESS':
            colorCode = '#00FF00' // Green
            break
        default:
            colorCode = '#FF0000' // Red
    }

    slackSend(color: colorCode, message: summary)
}
