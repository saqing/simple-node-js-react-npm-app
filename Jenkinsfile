pipeline {
    agent {
        docker {
            image 'node:6-alpine'
            args '-p 3000:3000'
        }
    }
    environment { 
        CI = 'true'
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Deliver') { 
            steps {
                sh './jenkins/scripts/deliver.sh' 
                input message: 'Finished using the web site? (Click "Proceed" to continue)' 
                sh './jenkins/scripts/kill.sh' 
            }
        }
    }
    
    post {
        success {
           def url = "https://oapi.dingtalk.com/robot/send?access_token=307639615ceb00d61fdffc34e61ad488b33b16859fde596a5b42714e61f2ce30"
           def body = """
                {
                     "msgtype": "text",
                     "text": {
                         "content": "The pipeline ${currentBuild.fullDisplayName} completed successfully."
                     }
                 }
                """

          httpRequest acceptType: 'APPLICATION_JSON', contentType: 'APPLICATION_JSON', httpMode: 'POST', requestBody: body, url: url
            
        }
    }
}
