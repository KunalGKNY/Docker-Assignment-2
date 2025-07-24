pipeline {
    agent none

    stages {
        stage('Checkout Code') {
            agent { label 'built-in' }
            steps {
                cleanWs()
                echo "Cloning repository..."
                git(
                    branch: 'main',
                    credentialsId: '441c171e-1195-4d3e-ac27-c9e1f15d1728',
                    url: 'https://github.com/KunalGKNY/Docker-Assignment-2.git'
                )
                stash name: 'html-files', includes: 'index.html'
            }
        }

        stage('Container on Slave1') {
            agent { label 'slave1' }
            options {
                skipDefaultCheckout() // Avoids git clone on slave
            }
            steps {
                cleanWs()
                unstash 'html-files'
                sh '''
                    docker inspect C1 >/dev/null 2>&1 || docker run -dp 80:80 --name C1 httpd
                    docker exec C1 rm -f /usr/local/apache2/htdocs/index.html || true
                    docker cp index.html C1:/usr/local/apache2/htdocs/
                    docker exec C1 chmod 644 /usr/local/apache2/htdocs/index.html
                '''
            }
        }

        stage('Container on Slave2') {
            agent { label 'slave2' }
            options {
                skipDefaultCheckout()
            }
            steps {
                cleanWs()
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
