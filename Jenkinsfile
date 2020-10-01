#!groovy
import groovy.json.JsonSlurperClassic
node {

    def HUB_ORG='kunal.sfdcpoc@wise-fox-96ck5y.com'
    def SFDC_HOST = 'https://login.salesforce.com'
    def JWT_KEY_CRED_ID = 'b1edc161-2e3b-404a-983e-5e3b956accf9'
    def CONNECTED_APP_CONSUMER_KEY='3MVG9n_HvETGhr3CaCrMP49P50NDILGbMex5g1H2oK8WwpblQ5Tp.wdXdmPVKqKMQBWn82br9g.jOi4M0Im.j'

    println 'KEY IS' 
    println JWT_KEY_CRED_ID
    println HUB_ORG
    println SFDC_HOST
    println CONNECTED_APP_CONSUMER_KEY
    def toolbelt = tool 'toolbelt'


    withCredentials([file(credentialsId: JWT_KEY_CRED_ID, variable: 'jwt_key_file')]) {
        stage('Authorize') {

			rc = command "${toolbelt}/sfdx force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwtkeyfile \"${jwt_key_file}\" --setdefaultdevhubusername --instanceurl ${SFDC_HOST}"
                if (rc != 0) {
                    error 'Salesforce dev hub org authorization failed.'
                }

			println rc

        }
    }
}
