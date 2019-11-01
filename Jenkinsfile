pipeline {
    agent {
        docker { image 'node:7-alpine' }
    }

    environment {
      pqr = ""
      slot = 0
      frm = ""
      frk = ""
      build = ""
      cust_key = "1"
      cust_id = 78
    }

    stages {
      stage('Setup') {
        steps {
          script {
                 echo 'Starting'
              node{
                yamlFile = readYaml file:'customerProperties.yaml'
              }
              
              customerName = yamlFile.customer.Name
              
              echo ${customerName}

            }
          }
        }
      }

    post {
      success {
        echo 'Completed'
      }
    }
}
