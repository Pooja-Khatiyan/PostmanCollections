pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Pooja-Khatiyan/PostmanCollections', branch: 'master'
            }
        }
        
        stage('Install Newman') {
            steps {
                bat '''
                    npm list -g newman || npm install -g newman
                    npm list -g newman-reporter-htmlextra || npm install -g newman-reporter-htmlextra
                '''
            }
        }
        
        stage('Build') {
            steps {
                echo "Building the application..."
            }
        }
        
        stage('Deploy to QA') {
            steps {
                echo "Deploying to QA environment..."
            }
        }
        
        stage('Run API Tests') {
    steps {
        bat '''
            if not exist results mkdir results
            newman run Lecture-7b-BookingApi.postman_collection.json ^
                -e BookingApi.postman_environment.json ^
                -n 2 ^
                -r htmlextra,cli ^
                --reporter-htmlextra-export results\\report.html ^
                --disable-unicode ^
                --color off
        '''
    }
}
        
        stage('Publish HTML Report') {
            steps {
                publishHTML([
                    allowMissing: false,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: 'results',
                    reportFiles: 'report.html',
                    reportName: 'Newman API Test Report',
                    reportTitles: 'API Test Results'
                ])
            }
        }
        
        stage('Deploy to PROD') {
            steps {
                echo "Deploying to PROD environment..."
            }
        }
    }
    
    post {
        always {
            echo "Pipeline execution completed"
        }
        success {
            echo "✅ All tests passed!"
        }
        failure {
            echo "❌ Tests failed!"
        }
    }
}