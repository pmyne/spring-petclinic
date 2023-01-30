pipeline {
  agent any
  stages {
    stage('Compile') {
      steps {
        sh './mvnw clean compile'
      }
    }

    stage('Static Analysis') {
      steps {
        sh '''./mvnw sonar:sonar \\
  -Dsonar.host.url=http://172.31.22.193:9000/ \\
  -Dsonar.projectKey=Petclinic \\
  -Dsonar.login=sqp_5f811d58cbb8e0ac360e0be47f779af92e51d187'''
      }
    }

    stage('Unit Test') {
      steps {
        sh '''./mvnw "-Dtest=**/petclinic/*/*.java" test
'''
      }
    }

    stage('Package') {
      steps {
        sh './mvnw package -DskipTests=true'
      }
    }

    stage('Deploy') {
      parallel {
        stage('Deploy') {
          steps {
            sh '''./mvnw spring-boot:run </dev/null &>/dev/null &
'''
          }
        }

        stage('Integration and Performance Test') {
          agent {
            node {
              label 'test'
            }

          }
          steps {
            sh '''./mvnw verify
'''
          }
        }

      }
    }

  }
}