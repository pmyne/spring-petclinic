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

  }
}