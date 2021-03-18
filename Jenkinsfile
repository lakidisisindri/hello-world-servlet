pipeline {
    agent any 
    tools { 
        maven 'maven' 
      
    }
stages { 
     
 stage('Preparation') { 
     steps {
// for display purposes

      // Get some code from a GitHub repository

      git 'https://github.com/lakidisisindri/hello-world-servlet.git'

      // Get the Maven tool.
     
 // ** NOTE: This 'M3' Maven tool must be configured
 
     // **       in the global configuration.   
     }
   }

   stage('Build') {
       steps {
       // Run the maven build

      //if (isUnix()) {
         sh 'mvn -Dmaven.test.failure.ignore=true install'
      //} 
      //else {
      //   bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
       }
//}
   }
 
  stage('Results') {
      steps {
      junit '**/target/surefire-reports/*.xml'
      archiveArtifacts 'target/*.war'
      }
 }
 stage('sonarqube') {
    environment {
        scannerHome = tool 'sonarqube'
    }
    steps {
        withSonarQubeEnv('sonarqube') {
            sh "${scannerHome}/bin/sonar-scanner"
        }
  //     timeout(time: 10, unit: 'MINUTES') {
  //     waitForQualityGate abortPipeline: true
    //    }
    }
}
     stage('Artifact upload') {
      steps {
    nexusArtifactUploader artifacts: [[artifactId: 'hello-world-servlet-example', classifier: '', file: '/var/lib/jenkins/workspace/HelloWorldServlet-Pipeline/target/helloworld.war', type: 'war']], credentialsId: 'nexus', groupId: 'com.geekcap.vmturbo', nexusUrl: '15.206.89.116:8081/nexus', nexusVersion: 'nexus2', protocol: 'http', repository: 'https://github.com/lakidisisindri/hello-world-servlet.git', version: '1.0-SNAPSHOT'
 }
    }
}
post {
        success {
            mail to:"lakidisisindri96@gmail.com", subject:"SUCCESS: ${currentBuild.fullDisplayName}", body: "Build success"
        }
        failure {
            mail to:"lakidisisindri96@gmail.com", subject:"FAILURE: ${currentBuild.fullDisplayName}", body: "Build failed"
        }
    }       
}
