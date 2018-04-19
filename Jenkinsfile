pipeline {
  agent any

  parameters {
    string(name: 'tomcat_dev', defaultValue: '52.29.92.233', description: 'Staging server')
    string(name: 'tomcat_prod', defaultValue: '18.197.95.235', description: 'Production server')
  }

  triggers {
    pollSCM('* * * * *')
  }

  stages {
    stage('Deployments') {
      parallel{
        stage('Deploy to staging'){
          steps {
            sh "scp -i /root/.ssh/id_rsa_jenkins **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
        } 
      } 
        stage('Deploy to production'){
          steps {
            sh "scp -i /root/.ssh/id_rsa_jenkins **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
          }
        }
      }
    }
  }
}
