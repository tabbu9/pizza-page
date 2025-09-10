pipeline {
    agent any

    environment {
        GIT_REPO = 'https://github.com/tabbu9/pizza-page.git'
        TOMCAT_HOME = '/opt/tomcat'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: "${GIT_REPO}"
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                script {
                    echo "Deploying Pizza Page to Tomcat..."
                    // Clear old ROOT app
                    sh "sudo rm -rf ${TOMCAT_HOME}/webapps/ROOT/*"
                    // Copy new files
                    sh "sudo cp -r * ${TOMCAT_HOME}/webapps/ROOT/"
                }
            }
        }

        stage('Restart Tomcat') {
            steps {
                script {
                    echo "Restarting Tomcat..."
                    sh "sudo systemctl restart tomcat"
                }
            }
        }
    }
}
