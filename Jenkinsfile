pipeline {
    agent any

    environment {
        GIT_AUTHOR_NAME = 'Jenkins CI'
        GIT_AUTHOR_EMAIL = 'ci@example.com'
        GIT_COMMITTER_NAME = 'Jenkins CI'
        GIT_COMMITTER_EMAIL = 'ci@example.com'
        APP_ENV = 'staging' // define this if you use it in reports or deploy
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', 
                    url: 'https://github.com/lena-biju/Capstone-project',
                    credentialsId: 'Lena#315051'
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean install -DskipTests'
            }
        }

        stage('Test') {
            steps {
                bat 'mvn test'
            }
        }

        stage('Publish Results') {
            steps {
                junit 'target/surefire-reports/*.xml'
            }
        }

        stage('Commit & Push Changes') {
            steps {
                script {
                    echo 'Checking for changes to push...'
                    bat """
                        git config user.email "jenkins@pipeline.com"
                        git config user.name "Jenkins CI"

                        git status
                        git add .

                        REM Commit only if there are changes
                        git diff --cached --quiet || git commit -m "Jenkins: Auto-commit after build"

                        REM Extract branch name from ref
                        for /f "tokens=2 delims=/" %%i in ("${env.GIT_BRANCH}") do set BRANCH=%%i

                        REM Push to GitHub
                        git push origin %BRANCH%
                    """
                }
            }
        }

        stage('Publish Reports') {
            steps {
                echo 'Publishing Cucumber and Extent reports...'

                cucumber fileIncludePattern: 'target/cucumber-reports/*.json',
                         buildStatus: 'UNSTABLE',
                         classifications: [[key: 'Env', value: "${env.APP_ENV}"]]

                publishHTML(target: [
                  reportDir: 'test-output/ExtentReport',
                  reportFiles: 'index.html',
                  reportName: 'Extent Report',
                  keepAll: true,
                  allowMissing: false,
                  alwaysLinkToLastBuild: true
                ])
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying to ${env.APP_ENV}..."
            }
        }
    }

    post {
        always { 
            echo "Build result: ${currentBuild.currentResult}" 
        }
        success { 
            echo ' Pipeline succeeded!' 
        }
        failure { 
            echo ' Pipeline failed! Check Jenkins logs and reports.' 
        }
    }
}
