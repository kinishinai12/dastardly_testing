// Jenkinsfile (Declarative Pipeline) for integration of Dastardly, from Burp Suite.

pipeline {
    agent any
    stages {
        stage ("Docker Pull Dastardly from Burp Suite container image") {
            steps {
                sh 'docker pull public.ecr.aws/portswigger/dastardly:latest'
            }
        }
        stage ("Docker run Dastardly from Burp Suite Scan") {
            steps {
                cleanWs()
                sh '''
                    docker run --user $(id -u) -v "/c/Program Files (x86)/Jenkins/workspace/dastardly_test:/c/Program Files (x86)/Jenkins/workspace/dastardly_test":rw \
                    -e BURP_START_URL=https://ginandjuice.shop/ \
                    -e BURP_REPORT_FILE_PATH="/c/Program Files (x86)/Jenkins/workspace/dastardly_test/dastardly-report.xml" \
                    public.ecr.aws/portswigger/dastardly:latest
                '''
            }
        }
    }
    post {
        always {
            junit allowEmptyResults: true, testResults: 'dastardly-report.xml'
        }
    }
}
