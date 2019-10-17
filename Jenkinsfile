pipeline {

     agent any
     triggers {
            pollSCM 'H/2 * * * 1-5'
     }
     parameters {
            choice choices: ['QA', 'UAT', 'PROD'], description: 'Pick any one option in dropdown box', name: 'ENVIRONMENT'
    }
    stages {
            stage('Checkout') {
                   steps{
                     checkout scm
                   }
            }
            stage('Build') {
                      steps {
                       sh '/home/mangesh/Software/apache-maven-3.6.1/bin/mvn install'
                      }
            }
            stage('Deployment') {
                      steps{
                         script {
                             
                           if(ENVIRONMENT == 'QA') {
                                sh 'sshpass -p "QAserver" scp target/Ghama.war QAserver@172.17.0.2:/home/QAserver/Teast/apache-tomcat-8.5.43/webapps '
                                sh 'sshpass -p "QAserver" ssh QAserver@172.17.0.2 "JAVA_HOME=/home/QAserver/Teast/jdk1.8.0_221" "/home/QAserver/Teast/apache-tomcat-8.5.43/bin/startup.sh"'
                           }
                           else if(ENVIRONMENT == 'PREPROD') {
                                sh 'sshpass -p "PreUser" scp target/Ghama.war PreUser@172.17.0.3:/home/PreUser/Pragati/apache-tomcat-8.5.43/webapps'
                                sh 'sshpass -p "PreUser" ssh PreUser@172.17.0.3 "JAVA_HOME=/home/PreUser/Pragati/jdk1.8.0_221" "/home/PreUser/Pragati/apache-tomcat-8.5.43/bin/startup.sh"
                           }
                         }
                      }
           }
    }

}
