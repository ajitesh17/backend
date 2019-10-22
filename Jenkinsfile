
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
        sh 'mvn sonar:sonar -Dsonar.password=admin -Dsonar.login=admin -Dsonar.host.url='+env.SONAR_URL+' -Dsonar.projectName=QA:ingproduct -Dsonar.projectKey=QA:com.hcl:ingproduct'
        timeout(time: 1, unit: 'HOURS') {
              script {
                    def qg = waitForQualityGate()
                    if (qg.status != 'OK') {
                        error "Pipeline aborted due to a quality gate failure:   ${qg.status}"
                    }
              }
    }
  }
 }
}
  # stage("Quality Gate"){
  # Dsonar.projectName=QA:ingproduct
  # Dsonar.projectKey=QA:com.hcl:ingproduct
  # timeout(time: 1, unit: 'MINUTES') {
   # def qg = waitForQualityGate()
   # if (qg.status != 'OK') {
   #     error "Pipeline aborted due to quality gate failure: ${qg.status}"
   #  }
  # }
 # }
# }
