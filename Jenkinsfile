pipeline {
  agent any
  stages {
    stage('Test')
    {
      steps{
        bat './gradlew test'
        error("failing the pipeline")
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
  
  
  
  post
  {
    success
    {
        notifyEvents message: 'BUILD SUCCESS', token: 'fLOrsgSMQXGmWzUzD4Gblw643QvvGc_e'
       mail body: 'BUILD SUCCESS', subject: 'BUILDING SUCCESSFULLY', to: 'jm_harchaoui@esi.dz'
    }
    failure
    {
       mail body: 'SUCCESS FAILED', subject: 'Build failed', to: 'jm_harchaoui@esi.dz'
    }
  }
  
  
  
  
}
