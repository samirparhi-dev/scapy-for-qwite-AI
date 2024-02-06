pipeline {
    agent any
    
    environment {
        GIT_REPO_URL = 'https://github.com/samirparhi-dev/scapy-for-qwite-AI.git'
    }
    
    stages {
        stage('Clone Repository') {
            steps {
                script {
                    git branch: 'master', url: GIT_REPO_URL
                }
            }
        }

        stage('Install pip') {
            steps {
                script {
                    // Install pip using the get-pip.py script
                    sh '''
                        curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
                        python get-pip.py
                    '''
                }
            }
        }
        
        stage('Build and Analyze Project') {
            steps {
                script {
                    dir('scapy-for-qwite-AI') {
                        sh '''
                            curl https://cdn.shiftleft.io/download/sl >/usr/local/bin/sl && chmod a+rx /usr/local/bin/sl
                            pip install -r requirements.txt
                            sl analyze
                        '''
                    }
                }
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
