pipeline {
  agent {
    docker {
      image 'maven:latest'
      args '-v /root/.m2:/root/.m2'
    }
  }

  parameters {
    string(name: 'staging_server', defaultValue: '18.184.0.134', description: 'Staging server')
    string(name: 'tomcat_prod', defaultValue: '18.197.95.235', description: 'Production server')
  }

  triggers {
    pollSCM('')
  }

  stages {
    stage('Build') {
      steps {
        sh 'mvn clean package'
      }
      post {
        success {
          echo 'Now archiving...'
          archiveArtifacts artifacts: '**/target/*.war'
        }
      }
    }
    stage('Deploy to staging'){
      steps {
        sshagent (credentials: ['ec2-staging']) {
          sh 'ssh -o StrictHostKeyChecking=no ec2-user@$18.184.0.134 uname -a'
        }  
      } 
    }
  }
}
