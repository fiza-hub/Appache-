pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM',
                    branches: [[name: '*/main']],
                    doGenerateSubmoduleConfigurations: false,
                    extensions: [],
                    userRemoteConfigs: [[
                        url: 'https://github.com/fiza-hub/Appache-'
                    ]]
                ])
            }
        }

        stage('Build') {
            steps {
                echo 'Building project...'
                // No build step required for HTML/CSS/JS
            }
        }
        
        stage('Deploy') {
            steps {
                sshagent(['apache']) { // Use the correct credential ID here
                    sh '''
                    # Transfer index.html and related files to Apache2 directory
                    scp -o StrictHostKeyChecking=no index.html ubuntu@13.60.223.61:/tmp/

                    # Move files to /var/www/html/ and restart Apache2
                    ssh -o StrictHostKeyChecking=no ubuntu@13.60.223.61 << EOF
                    sudo mv /tmp/index.html /var/www/html/
                    sudo systemctl restart apache2
                    EOF
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
