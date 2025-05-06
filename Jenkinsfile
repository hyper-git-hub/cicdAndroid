pipeline {
    agent any
 
    environment {
        GRADLE_BUILD_DIR = "./CICDdemo/app/build"
    }
 
    stages {
        stage('Checkout') {
            steps {
                echo "🔄 Checking out source code"
                git credentialsId: '0dae4b11-d489-4f03-a4df-070facbd0a17',
                    url: 'https://github.com/Khawaja-Abdul-Haleem-ios/cicdAndroid.git',
                    branch: 'main'
            }
        }
 
        stage('Install Dependencies') {
            steps {
                echo "📦 Skipping Fastlane install — already installed on agent"
                sh '''
                ./gradlew dependencies
                '''
            }
        }
 
        stage('Build & Deploy') {
            steps {
                echo "📱 Building and uploading Android app to Play Store..."
                dir('CICDdemo') {
                    sh 'fastlane beta'
                }
            }
        }
    }
 
    post {
        success {
            echo "✅ Android build uploaded to Play Store successfully!"
        }
        failure {
            echo "❌ Android build failed or upload error"
        }
    }
}
