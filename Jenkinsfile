pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            agent { label 'built-in' }
            steps {
                dir('/mnt/workspace1') {
                    echo "Cleaning workspace in /mnt/workspace1..."
                    sh 'rm -rf *'

                    echo "Cloning repository into /mnt/workspace1..."
                    git(
                        branch: 'main',
                        credentialsId: 'e4259c9b-8a98-4c29-8324-6ac34de2c01a',
                        url: 'https://github.com/KunalGKNY/Docker-Assignment-2.git'
                    )
                    stash name: 'html-files', includes: 'index.html'
                }
            }
        }

        stage('Docker Container Creations') {
            parallel {
                stage('Container on Slave 1') {
                    agent { label 'slave1' }
                    steps {
                        unstash 'html-files'
                        echo "Managing Docker container C1 on slave1..."
                        sh '''
                            docker stop C1 || true
                            docker rm C1 || true
                            docker run -dp 80:80 --name C1 httpd
                            docker cp index.html C1:/usr/local/apache2/htdocs/index.html
                            docker exec C1 chmod 644 /usr/local/apache2/htdocs/index.html
                            echo "C1 setup complete."
                        '''
                    }
                }

                stage('Container on Slave 2') {
                    agent { label 'slave2' }
                    steps {
                        unstash 'html-files'
                        echo "Managing Docker container C2 on slave2..."
                        sh '''
                            docker stop C2 || true
                            docker rm C2 || true
                            docker run -dp 8080:80 --name C2 httpd
                            docker cp index.html C2:/usr/local/apache2/htdocs/index.html
                            docker exec C2 chmod 644 /usr/local/apache2/htdocs/index.html
                            echo "C2 setup complete."
                        '''
                    }
                }
            }
        }
    }
}
