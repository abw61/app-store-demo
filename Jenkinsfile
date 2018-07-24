pipeline {
  agent {
    label 'maven-jdk-8'
  }
  stages {
    stage('Build') {
      steps {
        sh 'mvn clean source:jar package'
      }
    }
    stage('Browser Tests') {
      parallel {
        stage('Firefox') {
          steps {
            sh 'echo \'setting up selenium environment\''
            sh 'ping -c 5 localhost'
          }
        }
        stage('Safari') {
          steps {
            sh 'echo \'setting up selenium environment\''
            sh 'ping -c 8 localhost'
          }
        }
        stage('Chrome') {
          steps {
            sh 'echo \'setting up selenium environment\''
            sh 'ping -c 3 localhost'
          }
        }
        stage('Internet Explorer') {
          steps {
            sh 'echo \'setting up selenium environment\''
            sh 'ping -c 4 localhost'
          }
        }
      }
    }
    stage('Static Analysis') {
      parallel {
        stage('Static Analysis') {
          steps {
            sh 'mvn findbugs:findbugs'
          }
        }
      }
    }
    stage('Deploy') {
      steps {
          input message: 'User input required', ok: 'Release!',
            parameters: [choice(name: 'RELEASE_SCOPE', choices: 'patch\nminor\nmajor', description: 'What is the release scope?')]
          echo "env: ${env.RELEASE_SCOPE}"
          echo "params: ${params.RELEASE_SCOPE}"
          
          sh 'mvn source:jar package -Dmaven.test.skip'
      }
    }
  }
  post {
    always {
      junit '**/target/surefire-reports/TEST-*.xml'
      archive '**/target/*.jar'
      
    }
    
  }
}
