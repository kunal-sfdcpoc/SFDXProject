#!groovy
import groovy.json.JsonSlurperClassic
node {
	
	def PATH='force-app/main/default'
    def BUILD_NUMBER=env.BUILD_NUMBER
    def RUN_ARTIFACT_DIR="tests/${BUILD_NUMBER}"
    def SFDC_USERNAME

    def HUB_ORG='kunal.sfdcpoc@wise-fox-96ck5y.com'
    def SFDC_HOST = 'https://login.salesforce.com'
    def JWT_KEY_CRED_ID = '311cc457-7bf2-451e-a4ae-b5003244588e'
    def CONNECTED_APP_CONSUMER_KEY='3MVG9n_HvETGhr3CaCrMP49P50NDILGbMex5g1H2oK8WwpblQ5Tp.wdXdmPVKqKMQBWn82br9g.jOi4M0Im.j'

    println 'KEY IS' 
    println JWT_KEY_CRED_ID
    println HUB_ORG
    println SFDC_HOST
    println CONNECTED_APP_CONSUMER_KEY
    def toolbelt = tool 'toolbelt'

    stage('checkout source') {
        // when running in multi-branch job, one must issue this command
        checkout scm
    }

    withCredentials([file(credentialsId: JWT_KEY_CRED_ID, variable: 'jwt_key_file')]) {
        stage('Deploye Code') {
                 rc = bat returnStatus: true, script: "\"${toolbelt}\"/sfdx force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwtkeyfile \"${jwt_key_file}\" --setdefaultdevhubusername --instanceurl ${SFDC_HOST}"
            
            if (rc != 0) { error 'hub org authorization failed' }

			println rc
			

        }
		
		stage('Push To Org') {
                rcc = command "${toolbelt}/sfdx force:source:deploy --sourcepath ${PATH}" --json --loglevel fatal"
                if (rcc != 0) {
                    error 'Salesforce deploy failed.'
                }
				println rcc
            }
    }
}
