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
                       ls
                       $WORKSPACE/sl auth --org "52f01c37-e738-4261-b66e-b2bdec817cfb" --token "eyJhbGciOiJSUzUxMiIsInR5cCI6IkpXVCJ9.eyJpYXQiOjE3MDI5NTkyMTQsImlzcyI6IlNoaWZ0TGVmdCIsIm9yZ0lEIjoiNTJmMDFjMzctZTczOC00MjYxLWI2NmUtYjJiZGVjODE3Y2ZiIiwidXNlcklEIjoiMTdkNzc2NDktNGQxOC00Njg5LWI4YzMtMTczYzI1M2NhMzIyIiwic2NvcGVzIjpbInNlYXRzOndyaXRlIiwiZXh0ZW5kZWQiLCJhcGk6djIiLCJ1cGxvYWRzOndyaXRlIiwibG9nOndyaXRlIiwicGlwZWxpbmVzdGF0dXM6cmVhZCIsIm1ldHJpY3M6d3JpdGUiLCJwb2xpY2llczpjdXN0b21lciJdfQ.ZuLxdPEUQZysLrqI2AKn9C17l0iGVZrgOePgrymxmdtszgZwOVXDN9zYB8724K6mM35ZwP20wz2roSc0TLPBdqhv-ZpC5K75vtztzN4cRepTRty3uM8kRd1n-JTWlL1W4Z1BnJ84P5_XYBTmivt_H7Tqd2_cy44K8ymGgatG5dmGxefGeFjjw2WOd1OBkJrvOThCltykklM0YnOZ9cFqpvc34w3eVcMNrZMUk20ihJPdo4AyNonYK9R0kpR-Ftp4twr8yeyCuuPkE0Gy1K1zRzZAp63sKrIiJEtA5hoQ1CKdm7TUc_GHPOd_f2lMNvlN4dFaD_z82gCiU_yBNzCwCg" -y
                       
                       $WORKSPACE/sl analyze --app scapy --pythonsrc $WORKSPACE --diagnostic -y
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
