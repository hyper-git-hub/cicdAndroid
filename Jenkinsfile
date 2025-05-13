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
                echo "📦 Installing Gradle dependencies..."
                sh './gradlew dependencies'
            }
        }

        stage('Install Fastlane') {
            steps {
                echo "🚀 Installing Fastlane locally for this pipeline run..."
                sh '''
                    gem install --user-install fastlane -NV
                    export PATH="$HOME/.gem/ruby/3.2.0/bin:$PATH"
                    echo "✅ Fastlane version: $(fastlane --version)"
                '''
            }
        }

        stage('Build & Deploy') {
            steps {
                echo "📱 Building and uploading Android app to Play Store..."
                dir('CICDdemo') {
                    sh '''
                        export PATH="$HOME/.gem/ruby/3.2.0/bin:$PATH"
                        fastlane beta
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
