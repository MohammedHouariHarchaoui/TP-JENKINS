pipeline {
  agent any
  stages {
    stage('Test')
    {
      steps{
        bat './gradlew test'
      }
      post {
        always{
          archiveArtifacts  'build/reports/tests/**/*'
          cucumber buildStatus: 'UNSTABLE',
                reportTitle: 'Cucumber report',
                fileIncludePattern: 'target/report.json',
                trendsLimit: 10,
                classifications: [
                    [
                        'key': 'Browser',
                        'value': 'Brave'
                    ]
                ]
         
        }
      }
    }
    
    stage('Code Analysis')
    {
      steps{
      withSonarQubeEnv('sonarqube') 
          { 
            bat './gradlew sonar'
          }
        }
    }
    
  
    
  }
}
