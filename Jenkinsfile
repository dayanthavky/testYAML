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
              
              def d = [environment: 'a', customer: 'b', slot: 'c']
              def properties = readProperties file: 'gradle.properties'
              environment = properties['environment']  
              customer = properties['customer'] 
              slot = properties['slot'] 
              
              echo ${environment}
              echo ${customer}
              echo ${slot}

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
