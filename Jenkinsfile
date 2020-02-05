node {
   def mvnHome
   def scannerHome
   stage('SCM CheckOut') {
      //cleanWs disableDeferredWipeout: true, notFailBuild: true
      git branch: 'master', url: 'https://github.com/sridharsirineni/sridhar.github.git'
      mvnHome = tool 'M3'
     // def commit = sh(returnStdout: true, script: 'git log -1 --pretty=%B | cat')
      //def matcher = commit =~ ([a-zA-Z][a-zA-Z0-9_]+-[1-9][0-9]*)([^.]|\.[^0-9]|\.$|$)
      //print commit
      //print matcher
      scannerHome = tool 'SonarQubeScanner';
   }

   stage('Maven Build') {
      sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package"

   }

   stage('Test-JUnit') {
      sh "'${mvnHome}/bin/mvn' test surefire-report:report"
   }

   stage('Sonar Code Analysis') {
      withSonarQubeEnv('SonarQubeServer') {
        sh "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectName=mavenproject -Dsonar.projectKey=webapp -Dsonar.sources=src -Dsonar.java.binaries=target/"
      }
   }
   stage('Deploy Artifacts on NexusServer') {
     sh "mvn clean deploy"
   }

 }

