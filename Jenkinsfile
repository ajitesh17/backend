node('master'){
   stage('git checkout'){
                  git 'https://github.com/ajitesh17/INGPRODUCTS'
              }
   stage('java build'){
             sh 'mvn clean install sonar:sonar -Dsonar.password=admin -Dsonar.login=admin'
         }
   stage('Running java backend application'){
             sh 'export JENKINS_NODE_COOKIE=dontKillMe'
             sh 'nohup java -jar $WORKSPACE/target/*.jar &'
         }
}
