pipeline {
    agent any
    
    environment {
        GIT_REPO_URL = 'https://github.com/samirparhi-dev/scapy-for-qwite-AI.git'
        ARTIFACT_URL = 'https://github.com/samirparhi-dev/scapy-for-qwite-AI.git'

        SHIFTLEFT_DIAGNOSTIC=false
        SHIFTLEFT_ACCESS_TOKEN = credentials('SHIFTLEFT_ACCESS_TOKEN')
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
                       echo $SHIFTLEFT_DIAGNOSTIC
                       echo $GIT_REPO_URL
                       curl "https://bootstrap.pypa.io/get-pip.py" -o "get-pip.py"
                       python3 get-pip.py --user
                       curl https://cdn.shiftleft.io/download/sl > $WORKSPACE/sl && chmod a+rx $WORKSPACE/sl
                       ls
                       $WORKSPACE/sl analyze --app scapy --pythonsrc $WORKSPACE
                    '''
                }
            }
        }


    }
}
