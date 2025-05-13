pipeline {
    agent any

    environment {
        GRADLE_BUILD_DIR = "./CICDdemo/app/build"
        PATH = "/usr/local/bin:$PATH"  // Ensure Fastlane is in PATH globally
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
                echo "📦 Installing Gradle dependencies..."
                sh '''
                    ./gradlew dependencies
                '''
            }
        }

        stage('Ensure Fastlane is Installed') {
            steps {
                echo "🚀 Checking Fastlane installation..."
                sh '''
                    if ! command -v fastlane &> /dev/null; then
                        echo "Fastlane not found. Installing..."
                        sudo gem install fastlane -NV
                    else
                        echo "✅ Fastlane is already installed."
                    fi
                '''
            }
        }

        stage('Build & Deploy') {
            steps {
                echo "📱 Building and uploading Android app to Play Store..."
                dir('CICDdemo') {
                    sh '''
                        export PATH=/usr/local/bin:$PATH
                        /usr/local/bin/fastlane beta
                    '''
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
