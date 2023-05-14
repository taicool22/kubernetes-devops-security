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
              sh "mvn clean verify sonar:sonar -Dsonar.projectKey=numeric-application1 -Dsonar.projectName='numeric-application' -Dsonar.host.url=http://taicool-demo.eastus.cloudapp.azure.com:9000 -Dsonar.token=sqp_7c75e169873f94f843ee950027de9584b80611b6"
            } 
            timeout(time: 2, unit: 'MINUTES') {
              script {
                waitForQualityGate abortPipeline: true
              }
            }
          }          
    }
}