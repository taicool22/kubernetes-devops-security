pipeline {
  agent any

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar' //so that they can be downloaded later
            }
        }   
        
      stage('Unit Tests') {
            steps {
              sh "mvn test"
              archive 'target/*.jar' //so that they can be downloaded later
            }

        }   
      stage('Unit Tests and Jacoco') {
            steps {
              sh "mvn test"
            }
            post {
                always {
                   junit 'target/surefire-reports/*.xml'
                   jacoco execPattern: 'target/jacoco.exec'
                }
            } 
         }   

      stage('sonarQube - SAST') {
            steps {
              withSonarQubeEnv('SonarQube') {
              sh "mvn clean verify sonar:sonar -Dsonar.projectKey=numeric-application -Dsonar.projectName='numeric-application' -Dsonar.host.url=http://taicool-demo.eastus.cloudapp.azure.com:9000 -Dsonar.token=sqp_ec96e188cf9b3d3a9a708263d6103711c3b67179"
              }    
              timeout(time: 1, unit: 'HOURS') {
                    // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
                    // true = set pipeline to UNSTABLE, false = don't
                    waitForQualityGate abortPipeline: false
              }
          } 
      }         
    }
}