pipeline {
    agent any

    environment {
        ANDROID_HOME = "${HOME}/Android/sdk"
        JAVA_HOME = "/usr/lib/jvm/java-11-openjdk-amd64" // adjust if different
        GRADLE_USER_HOME = "${HOME}/.gradle"
    }

    tools {
        gradle 'gradle-7' // Make sure this tool is configured in Jenkins
    }

    stages {
        stage('Clean Workspace') {
            steps {
                deleteDir()
            }
        }

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build APK & AAB') {
            steps {
                sh './gradlew clean'
                sh './gradlew assembleRelease'
                sh './gradlew bundleRelease'
            }
        }

        stage('Install Fastlane') {
            steps {
                sh 'gem install bundler'
                sh 'bundle install' // uses Gemfile and Gemfile.lock
            }
        }

        stage('Publish to Play Store') {
            steps {
                withCredentials([
                    file(credentialsId: 'play-store-key', variable: 'PLAY_STORE_JSON'),
                    file(credentialsId: 'upload-keystore', variable: 'UPLOAD_KEYSTORE')
                ]) {
                    sh '''
                        export SUPPLY_JSON_KEY=${PLAY_STORE_JSON}
                        export KEYSTORE_PATH=${UPLOAD_KEYSTORE}
                        bundle exec fastlane android deploy
                    '''
                }
            }
        }
    }

    post {
        success {
            echo '✅ App successfully published to Google Play!'
        }
        failure {
            echo '❌ Build failed.'
        }
    }
}
