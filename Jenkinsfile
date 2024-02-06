pipeline {
    agent any
    
    environment {
        GIT_REPO_URL = 'https://github.com/samirparhi-dev/scapy-for-qwite-AI.git'
        ARTIFACT_URL = 'https://github.com/samirparhi-dev/scapy-for-qwite-AI.git'
    }
    
    stages {
        stage('Clone Repository') {
            steps {
                script {
                    git branch: 'master', url: GIT_REPO_URL
                }
            }
        }

        stage('Install pip & Build') {
            steps {
                script {
                    // Install pip using the get-pip.py script
                    sh '''
                       echo $WORKSPACE
                       curl "https://bootstrap.pypa.io/get-pip.py" -o "get-pip.py"
                       python3 get-pip.py --user
                       curl https://cdn.shiftleft.io/download/sl > $WORKSPACE/sl && chmod a+rx $WORKSPACE/sl
                       sudo mv $HOME/sl /usr/local/bin/
                       ls
                       sl analyze --app scapy --pythonsrc $WORKSPACE
                    '''
                }
            }
        }

        stage('Ocular Scan') {
            steps {
               slOcularScan(
                  artifact: "ARTIFACT_URL",
                  threadFix: false,
                  debug: true,
                  ocularArgs: "-J-Xmx4000m",
                  orgId: "ORG_ID",
                  accessToken: "SHIFTLEFT_ACCESS_TOKEN"
               )
            }
         }
    }
}
