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
                       curl "https://bootstrap.pypa.io/get-pip.py" -o "get-pip.py"
                       python3 get-pip.py --user
                       pip install -r requirements.txt
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
    
    post {
        always {
              script {
                // Clean up or perform post-build actions
                echo "Post-build actions:"
                
                // Archive artifacts
                archiveArtifacts 'WORKSPACE/scapy'
                
                // Send email notification
                emailext subject: 'Build Status',
                          body: 'The build is complete. Check the console output for details.',
                          to: 'samirparhi@gmail.com'
            }
        }
    }
}
