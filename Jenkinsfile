@Library('github.com/rupalibehera/osio-pipeline@pod_template') _
def utils = new io.openshift.Utils()

osio {
  
  config runtime: 'java', version: '1.8'
  echo "ci................................"
  ci {
    namespaceToRunTest = testNamespace()
     integrationTestCmd = "mvn verify integration-test -Dnamespace.use.current=false -Dnamespace.use.existing=${namespaceToRunTest} -Dit.test=*IT -DfailIfNoTests=false -DenableImageStreamDetection=true -Popenshift,openshift-it"
     runTest commands: integrationTestCmd
    //spawn image: config.runtime(), version: config.version(), commands: integrationTestCmd, env: ["key":"value", "key2":"value2"]
  }

  cd {

    def resources = processTemplate(params: [
          release_version: "1.0.${env.BUILD_NUMBER}"
    ])
    echo "CD build"

    build resources: resources
    deploy resources: resources, env: 'stage'
    deploy resources: resources, env: 'run', approval: 'manual'

  }
}
