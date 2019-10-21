node('master'){
   stage('git checkout'){
                  git 'https://github.com/ajitesh17/INGPRODUCTS'
              }
   stage('java build'){
             sh 'mvn clean install sonar:sonar -Dsonar.password=admin -Dsonar.login=admin'
         }
   stage('Running java backend application'){
             sh 'export JENKINS_NODE_COOKIE=dontKillMe ;nohup java -Dspring.profiles.active=dev -jar $WORKSPACE/target/*.jar &'
         }
   stage("Quality Gate"){
    timeout(time: 1, unit: 'HOURS') { // Just in case something goes wrong, pipeline will be killed after a timeout
    def qg = waitForQualityGate() // Reuse taskId previously collected by withSonarQubeEnv
    if (qg.status != 'OK') {
        error "Pipeline aborted due to quality gate failure: ${qg.status}"
    }
  }
 }
}
