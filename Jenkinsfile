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
             
              def yamlFile = readYaml file:'customerProperties.yaml'
              Name = yamlFile.customer.Name
              sh "echo 'Name=${Name}'"
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
