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
      stage('Mutation Test - PIT') {
            steps {
              sh "mvn test-compile org.pitest:pitest-maven:mutationCoverage"
            }
            post {
                always {
                   pitmutation mutationStatsFile: '**/target/pit-reports/**/mutations.xml'
                }         
            }
        }              
    }
}