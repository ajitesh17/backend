node('master'){
   stage('git checkout'){
                  git 'https://github.com/ajitesh17/INGPRODUCTS'
              }
   stage('java build'){
             sh 'mvn clean install'
         }
   stage('Running java backend application'){
             sh 'export JENKINS_NODE_COOKIE=dontKillMe ;nohup java -Dspring.profiles.active=dev -jar $WORKSPACE/target/*.jar &'
         }
   stage('SonarQube analysis') {
    withSonarQubeEnv('sonarqube') {
        sh 'mvn sonar:sonar -Dsonar.password=admin -Dsonar.login=admin'
    }
  }
  stage("Quality Gate"){
   timeout(time: 1, unit: 'MINUTES') {
    def qg = waitForQualityGate()
    if (qg.status != 'OK') {
        error "Pipeline aborted due to quality gate failure: ${qg.status}"
    }
  }
 }
}
