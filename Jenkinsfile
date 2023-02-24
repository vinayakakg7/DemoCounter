pipeline {
    agent any

   // @Grab(group='org.apache.tomcat.maven', module='tomcat-maven-plugin', version='3.2')

    tools {
        maven 'maven'
        jdk 'Java'
    }
    
    environment {
        //SONAR_HOST_URL = 'http://localhost:9000'
       // NEXUS_URL = '13.232.22.30:8081'
        GIT_REPO = 'https://github.com/vinayakakg7/DemoCounter.git'
        GIT_BRANCH = 'main'
      //  NEXUS_SNAPSHOT_REPO = 'demo_snapshot'
      //  NEXUS_RELEASE_REPO = 'demo_release'
        TOMCAT_URL = "http://13.235.75.30:8084"
        //TOMCAT_USER = "admin"
        //TOMCAT_PASSWORD = "P@ssw0rdkgv1"
        //WAR_FILE = "**/*.jar"

    }
    
    stages {
        stage('Clone Git repository') {
            steps {
                git branch: GIT_BRANCH, url: GIT_REPO
            }
        }
        
        stage('Build and test using Maven') {
            steps {
                bat 'mvn clean install -DskipTests=true'
            }
        }
        
        stage('Run SonarQube analysis') {
          steps {

             script{
                withSonarQubeEnv(credentialsId: 'sonarapi') {
                    bat 'mvn clean package sonar:sonar'
               }
           }
           }
       }
        
 //       stage('Check quality gate status') {
 //           steps {
 //               script {
  //                  def qg = waitForQualityGate()
  //                  if (qg.status != 'OK') {
  //                      error "Pipeline aborted due to quality gate failure: ${qg.status}"
  //                  }
   //             }
   //         }
   //     }
        
      //  stage('Upload JAR to Nexus repository') {

        //    steps {

          //      script {
            //        def pom = readMavenPom file: 'pom.xml'
              //      def version = pom.version
                //    env.version = version
                  //  def snapshot = version.endsWith('-SNAPSHOT')
                    
                    //def repo = snapshot ? NEXUS_SNAPSHOT_REPO : NEXUS_RELEASE_REPO
                   // env.repo = repo
                  
                    //def groupId = pom.groupId
                   // env.groupId = groupId

        
                   // nexusArtifactUploader artifacts: [
                     //       [artifactId: 'springboot', classifier: '', file: 'target/Uber.jar', type: 'jar']
                       //     ], 
                         //   credentialsId: 'nexus_cred', 
                           // groupId: "${env.groupId}", 
                            //nexusUrl: '13.232.22.30:8081',
                            //nexusVersion: 'nexus3', 
                            //protocol: 'http',
                            //repository: "${env.repo}", 
                           // version: "${env.version}"

                    //}
             
                    
                    //}
               // }
    // stage('Deploy') {
      //     steps {
      //          deploy(
     //               adapters: [
     //                   tomcat9(credentialsId: 'tomcatcred', url: 'http://localhost:8082/manager/text')
    //              ],
    //                contextPath: 'Uber/',
    //                war: '**/*.Jar'
     //           )
          

       
      //  sshagent(['Tomcat_User']) {
      //    cmd 'ssh -o StrictHostKeyChecking=no ubuntu@13.233.120.253 "sudo systemctl tomcat stop && sudo rm -rf /opt/tomcat/webapps/*.jar && sudo cp "target\\*.jar" "/opt/tomcat/webapps/" && sudo systemctl tomcat start"'
      //    }
            
          stage('Deploy') {
      steps {
        bat 'copy "target\\*.jar" "C:\\Program Files\\Apache Software Foundation\\Tomcat 9.0\\webapps"'
        bat 'curl -u admin:P@ssw0rdkgv1 "http://localhost:8082/manager/text?path=/uber"'
   //     bat 'curl -u manager:P@ssw0rdkgv1 "http://13.233.120.253:8084/manager/text?path=/uber"'

      }
    }  

    stage('Docker image build'){
      steps{
        script{
          bat 'docker image build -t $JOB_NAME:V1.$BUILD_ID .'
          bat 'docker image tag  $JOB_NAME:V1.$BUILD_ID vinayakakg7/$JOB_NAME:V1.$BUILD_ID'
          bat 'docker image tag  $JOB_NAME:V1.$BUILD_ID vinayakakg7/$JOB_NAME:V1.latest'
        }
      }
    }
    stage('Push image to DockerHub'){
      steps{
        script{
          withCredentials([string(credentialsId: 'Docker_Credentials', variable: 'Docker_Cred')]) {
            bat 'docker login -u vinayakakg7 -p ${Docker_Cred}'
             bat 'docker image push vinayakakg7/$JOB_NAME:V1.$BUILD_ID'
              bat 'docker image push vinayakakg7/$JOB_NAME:latest'

    
}

        }
      }
    }

         
            }
          }
    
      
    
  


    


