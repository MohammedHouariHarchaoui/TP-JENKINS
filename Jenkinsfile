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
      withSonarQubeEnv('Sonarqube') 
          { 
            bat './gradlew sonar'
          }
        }
    }
    
        stage('Code Quality')
    {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                waitForQualityGate abortPipeline: true
                }
            }
    }
    
        stage('Build')
    {
            steps 
                {
                  bat './gradlew build'
                }
            post {
          success {
            bat './gradlew javadoc'
            archiveArtifacts artifacts:  'build/libs/*,  build/docs/**/*'
          }
        }
    }
    
         stage('Deploy')
    {
            steps {
                bat './gradlew publish'
                }
    }
    
  }
}
