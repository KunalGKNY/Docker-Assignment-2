pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            agent { label 'built-in' }
            steps {
                dir('/mnt/workspace1') {
                    echo "Cleaning workspace..."
                    sh 'rm -rf /mnt/workspace1/*'

                    echo "Cloning repository..."
                    git(
                        branch: 'main',
                        credentialsId: 'e4259c9b-8a98-4c29-8324-6ac34de2c01a',
                        url: 'https://github.com/KunalGKNY/Docker-Multibranch.git'
                    )

                    stash name: 'html-files', includes: '*.html'
                }
            }
        }

        stage('Docker container creations') {
            parallel {
                containerOnSlave1: {
                    agent { label 'slave1' }
                    stages {
                        stage('Create C1') {
                            steps {
                                unstash 'html-files'
                                sh '''
                                    docker inspect C1 >/dev/null 2>&1 || docker run -dp 80:80 --name C1 httpd
                                    docker exec C1 rm -f /usr/local/apache2/htdocs/index.html || true
                                    docker cp index.html C1:/usr/local/apache2/htdocs/
                                    docker exec C1 chmod 644 /usr/local/apache2/htdocs/index.html
                                '''
                            }
                        }
                    }
                }

                containerOnSlave2: {
                    agent { label 'slave2' }
                    stages {
                        stage('Create C2') {
                            steps {
                                unstash 'html-files'
                                sh '''
                                    docker inspect C2 >/dev/null 2>&1 || docker run -dp 8080:80 --name C2 httpd
                                    docker exec C2 rm -f /usr/local/apache2/htdocs/index.html || true
                                    docker cp index.html C2:/usr/local/apache2/htdocs/
                                    docker exec C2 chmod 644 /usr/local/apache2/htdocs/index.html
                                '''
                            }
                        }
                    }
                }
            }
        }
    }
}
