pipeline {
   // agent { label 'macos'}

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
