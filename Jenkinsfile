pipeline {
    agent { label 'macos'}

    options {
      buildDiscarder(logRotator(numToKeepStr: '30'))
      ansiColor('xterm')
    }

    parameters {
      choice(name: 'environment', choices: ['release', 'staging', 'development'], description: '')
      booleanParam(name: 'generate_asset', defaultValue: true, description: 'enable if asset generation is required')

      choice(name: 'crypto_mode', choices: ['nonpqr', 'pqr'], description: '')
      choice(name: 'customer', choices: ['78', '7801','7802','7803','7804','7805','7806','7807','7808','7809','7810','7811','7812','7813', '188', '200', '999', '1001', '1002', '1003', '1004', '1008', '1009', '1010', '1011', '1012', '1013', '1014', '1015', '1016', '1017', '1018', '1019', '1020', '1021', '1022', '1023', '1024', '1025', '1026', '1027', '1028', '1029', '1030', '1031', '1032', '1033', '1034', '1035', '1036', '1037', '1038', '1039', '1040', '1041', '1042', '1043', '1044', '1045', '1046', '1047', '1048', '1049', '1050', '1051', '1052', '1053','1054', '1055', '1056', '1057', '1058', '1059'], description: '')
      choice(name: 'customer_environment', choices: ['UAT', 'POC', 'PROD'], description: '')

      string(name: 'toolchain_version', defaultValue: 'latest.release', description: '')
      string(name: 'signature_version', defaultValue: 'latest.release', description: '')
      string(name: 'firmware_version', defaultValue: 'latest.release', description: '')

      string(name: 'processor_android_version', defaultValue: 'latest.release', description: '')
      string(name: 'processor_ios_version', defaultValue: 'latest.release', description: '')

      string(name: 'securefileio_android_version', defaultValue: 'latest.release', description: '')
      string(name: 'securefileio_ios_version', defaultValue: 'latest.release', description: '')

      string(name: 'vguard_android_version', defaultValue: 'latest.release', description: '')
      string(name: 'vguard_ios_version', defaultValue: 'latest.release', description: '')

      string(name: 'vtap_android_version', defaultValue: 'latest.release', description: '')
      string(name: 'vtap_ios_version', defaultValue: 'latest.release', description: '')

      string(name: 'otp_android_version', defaultValue: 'latest.release', description: '')
      string(name: 'otp_ios_version', defaultValue: 'latest.release', description: '')

      string(name: 'pki_android_version', defaultValue: 'latest.release', description: '')
      string(name: 'pki_ios_version', defaultValue: 'latest.release', description: '')

      string(name: 'cryptota_android_version', defaultValue: 'latest.release', description: '')
      string(name: 'cryptota_ios_version', defaultValue: 'latest.release', description: '')

      string(name: 'ekyc_android_version', defaultValue: 'latest.release', description: '')
      string(name: 'ekyc_ios_version', defaultValue: 'latest.release', description: '')

      booleanParam(name: 'vos_sdk', defaultValue: false, description: '')
      booleanParam(name: 'vguard_sdk', defaultValue: true, description: '')
      booleanParam(name: 'vtap_sdk', defaultValue: true, description: '')
      booleanParam(name: 'ekyc_sdk', defaultValue: false, description: '')
    }

    tools {
      gradle 'latest'
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
            if(generate_asset)
            {
              pqr = params.crypto_mode == 'pqr' ? 'pqr': ''
              build = params.generate_asset ? 'build' : ''
              sh 'rm -rf customer_repo || true'
              if(params.customer_environment == "POC"){
                cust_id = 78
              }
              else {
                cust_id = params.customer
              }
              dir("customer_repo/${cust_id}"){
                git branch: "${params.customer_environment}", credentialsId: '874de463-ca79-45a6-a5f6-0779213d3673', url: "git@svn.intranet.v-key.com:customer/${params.customer}"
                def customer_prop_defaults = [hsm_slot: '4', FRK: '', FRM: '', cust_key: '1']
                def properties = readProperties file: 'customer.properties', defaults: customer_prop_defaults
                slot = properties["hsm_slot"]
                cust_key = properties["cust_key"]
                frk = properties["FRK"]
                frm = properties["FRM"]
              }
            }
          }

          sh "echo 'environment=${params.environment}' > gradle.properties"
          sh "echo 'crypto_mode=${pqr}' >> gradle.properties"
          sh "echo 'customer=${cust_id}' >> gradle.properties"
          sh "echo 'slot=${slot}' >> gradle.properties"
          sh "echo 'cust_key=${cust_key}' >> gradle.properties"
          sh "echo 'vos_sdk=${params.vos_sdk}' >> gradle.properties"
          sh "echo 'vguard_sdk=${params.vguard_sdk}' >> gradle.properties"
          sh "echo 'vtap_sdk=${params.vtap_sdk}' >> gradle.properties"
          sh "echo 'ekyc_sdk=${params.ekyc_sdk}' >> gradle.properties"
          sh "echo 'generate_asset=${params.generate_asset}' >> gradle.properties"
          sh "echo 'FRM=${frm}' >> gradle.properties"
          sh "echo 'FRK=${frk}' >> gradle.properties"

          sh "echo 'toolchain_version=${params.toolchain_version}' >> gradle.properties"
          sh "echo 'firmware_version=${params.firmware_version}' >> gradle.properties"
          sh "echo 'signature_version=${params.signature_version}' >> gradle.properties"

          sh "echo 'processor_android_version=${params.processor_android_version}' >> gradle.properties"
          sh "echo 'processor_ios_version=${params.processor_ios_version}' >> gradle.properties"

          sh "echo 'securefileio_android_version=${params.securefileio_android_version}' >> gradle.properties"
          sh "echo 'securefileio_ios_version=${params.securefileio_ios_version}' >> gradle.properties"

          sh "echo 'vguard_android_version=${params.vguard_android_version}' >> gradle.properties"
          sh "echo 'vguard_ios_version=${params.vguard_ios_version}' >> gradle.properties"

          sh "echo 'vtap_android_version=${params.vtap_android_version}' >> gradle.properties"
          sh "echo 'vtap_ios_version=${params.vtap_ios_version}' >> gradle.properties"

          sh "echo 'otp_android_version=${params.otp_android_version}' >> gradle.properties"
          sh "echo 'otp_ios_version=${params.otp_ios_version}' >> gradle.properties"

          sh "echo 'pki_android_version=${params.pki_android_version}' >> gradle.properties"
          sh "echo 'pki_ios_version=${params.pki_ios_version}' >> gradle.properties"

          sh "echo 'cryptota_android_version=${params.cryptota_android_version}' >> gradle.properties"
          sh "echo 'cryptota_ios_version=${params.cryptota_ios_version}' >> gradle.properties"

          sh "echo 'ekyc_android_version=${params.ekyc_android_version}' >> gradle.properties"
          sh "echo 'ekyc_ios_version=${params.ekyc_ios_version}' >> gradle.properties"

          sh "cat gradle.properties"
          sh "gradle clean dependencies --refresh-dependencies | grep .vkey. | sort -u"

          sh "printenv"
          sh "gradle resolveSDK ${build}"
        }
      }
      stage('Archive Asset') {
        when {
          expression { return params.generate_asset }
        }
        steps {
          sh "gradle zipAsset"
          archiveArtifacts artifacts: 'output/release-asset-package.zip', fingerprint: true
        }
      }
      stage('Archive SDK') {
        when {
          expression { return (params.vos_sdk||params.vguard_sdk||params.vtap_sdk||params.ekyc_sdk) }
        }
        steps {
          archiveArtifacts artifacts: 'output/*-sdk-package.zip', fingerprint: true
        }
      }
    }

    post {
      success {
        emailext attachLog: true, body: '${DEFAULT_CONTENT}', recipientProviders: [requestor()], subject: '${DEFAULT_SUBJECT}'
        cleanWs cleanWhenFailure: false, cleanWhenUnstable: false
      }
      always {
        script {
          currentBuild.displayName = "#${BUILD_NUMBER}.${params.customer}-${params.customer_environment}" + (params.crypto_mode.isEmpty() ? '' : "-${params.crypto_mode}") + "@${slot}_${params.environment}"
          currentBuild.description = "firmware=${params.firmware_version}\n processor=${params.processor_version}\n sdk = " + ((params.vos_sdk||params.vguard_sdk||params.vtap_sdk||params.ekyc_sdk) ? "true": "false")
        }
      }
    }
}
