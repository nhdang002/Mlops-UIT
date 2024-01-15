import groovy.transform.Field

@Field 
TOOL_FAILED = [false, false, false, false, false]
COMMIT_ID = ''
COMMIT_REPO = ''
SOURCE = 'git-source'
DOCKER_IMAGE = '10.0.0.20:30083/web-app'
WEB_MANAGER_ADDR = 'http://dashboard-backend:3000/api'


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
        COMMIT_ID = env.gitlabAfter
        COMMIT_REPO = env.gitlabSourceRepoName
        git branch: 'develop', credentialsId: 'gitlab-root', url: 'https://github.com/nhdang002/Mlops-UIT.git'
        stash includes: '**', name: "${SOURCE}"
        webHookCheck('start')
    }
}


