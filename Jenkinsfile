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

                    // Ensure ROOT exists
                    sh "sudo mkdir -p ${TOMCAT_HOME}/webapps/ROOT"

                    // Clean old deployment
                    sh "sudo rm -rf ${TOMCAT_HOME}/webapps/ROOT/*"

                    // Copy project files (ignore Jenkinsfile & README.md)
                    sh """
                        if [ -f index.html ]; then sudo cp index.html ${TOMCAT_HOME}/webapps/ROOT/; fi
                        if [ -d css ]; then sudo cp -r css ${TOMCAT_HOME}/webapps/ROOT/; fi
                        if [ -d js ]; then sudo cp -r js ${TOMCAT_HOME}/webapps/ROOT/; fi
                        if [ -d images ]; then sudo cp -r images ${TOMCAT_HOME}/webapps/ROOT/; fi
                    """
                }
            }
        }

        stage('Restart Tomcat') {
            steps {
                script {
                    echo "Restarting Tomcat..."
                    sh """
                        # Stop Tomcat if running
                        if [ -f ${TOMCAT_HOME}/bin/shutdown.sh ]; then
                            sudo ${TOMCAT_HOME}/bin/shutdown.sh || true
                        fi

                        # Start Tomcat
                        sudo ${TOMCAT_HOME}/bin/startup.sh
                    """
                }
            }
        }
    }
}
