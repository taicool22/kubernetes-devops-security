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
              withSonarQubeEnv('sonarQube') {
              sh "mvn clean verify sonar:sonar -Dsonar.projectKey=numeric-application -Dsonar.projectName='numeric-application' -Dsonar.host.url=http://taicool-demo.eastus.cloudapp.azure.com:9000 -Dsonar.token=sqp_ec96e188cf9b3d3a9a708263d6103711c3b67179"
              }    
            } 
         }
      stage('Vulnerability Scan - Docker') {
            steps {
              sh "mvn dependency-check:check"
            }  
            post {
              always {
                dependencyCheckPulisher pattern: 'target/dependency-check-report.xml'
              }
            }
    
      }
    }
}