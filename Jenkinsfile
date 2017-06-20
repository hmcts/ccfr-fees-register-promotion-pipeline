#!groovy

@Library('Reform')
import uk.gov.hmcts.Artifactory
import uk.gov.hmcts.Packager
import uk.gov.hmcts.RPMTagger

def artifactory = new Artifactory(this)
def packager = new Packager(this, 'cc')

properties([
        [$class: 'GithubProjectProperty', displayName: 'fees-register promotion pipeline', projectUrlStr: 'https://git.reform.hmcts.net/fees-register/fees-register-promotion-pipeline'],
        parameters([string(description: 'RPM Version', name: 'rpmVersion')])
])

node {
    try {
        stage('Tag testing passed'){
            new RPMTagger(this, 'fees-register-api', packager.rpmName('fees-register-api', params.idamApiRpmVersion), 'cc-local').tagTestingPassedOn('test')
        }

        stage('Promote to Prod') {
            deleteDir()
            artifactory.promoteLatestEligibleRPMtoProductionRepo('cc', 'fees-register-api')
        }
    } catch (Throwable err) {
        notifyBuildFailure channel: '#cc_tech'
        throw err
    }
}