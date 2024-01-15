pipeline {
    agent any
    stages {
        //Initialize scanning results of the commit-id
        def webHookCheck(String check){
            sh returnStatus: true, script: """
            curl '${WEB_MANAGER_ADDR}/commit?id=${COMMIT_ID}&repo=${COMMIT_REPO}&check=${check}'
            """
        }

        //Update status of scanning result
        def webHookScan(String tool, int ret, int index){
            String status
            if (ret != 0){
                status = 'fail'
                TOOL_FAILED[index] = true
                sh returnStatus: true, script: """
                curl '${WEB_MANAGER_ADDR}/commit?id=${COMMIT_ID}&tool=${tool}&status=${status}'
                """
            } else {
                status = 'pass'
            }
            sh returnStatus: true, script: """
            curl '${WEB_MANAGER_ADDR}/commit?id=${COMMIT_ID}&tool=${tool}&status=${status}'
            """   
        }

        node ('Main') {
            stage('Git clone and setup'){
                git branch: 'main',  url: 'https://github.com/nhdang002/Mlops-UIT.git'
                stash includes: '**', name: "${SOURCE}"
                webHookCheck('start')
            }
        }

    }
}