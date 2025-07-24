pipeline {
    agent any

    stages {
        stage('Checkout Code') {
		  agent { label 'built-in'}
            steps {
                dir('/mnt/workspace1/') {
                    echo "Cleaning workspace..."
                    sh 'rm -rf /mnt/workspace1/*'

                    echo "Cloning repository..."
                    git(
                        branch: 'main',
                        credentialsId: 'e4259c9b-8a98-4c29-8324-6ac34de2c01a',
                        url: 'https://github.com/KunalGKNY/Docker-Assignment-2.git'
                    )
					stash name: 'html-files', includes: 'index.html'
                }
            }
        }

 

        stage('Docker container creations') {
		  agent { label 'slave1'}
            steps {
			
			      dir('/mnt/workspace1/') {
				  
				     sh 'rm -rf /mnt/workspace1/*'
				     unstash 'html-files'
					
					sh '''

                    # Create container C1 if not exists
                    docker inspect C1 >/dev/null 2>&1 || docker run -dp 80:80 --name C1 httpd
					
					 docker exec C1 rm -f /usr/local/apache2/htdocs/index.html || true
					 
					 docker cp /mnt/workspace1/index.html C1:/usr/local/apache2/htdocs
					 
                    docker exec C1 chmod 644 /usr/local/apache2/htdocs/index.html
					 
					 
					
                    '''
				  
				  }
            }
        }

        
		stage('Docker container creations') {
		  agent { label 'slave2'}
            steps {
			
			      dir('/mnt/workspace1/') {
				  
				     sh 'rm -rf /mnt/workspace1/*'
				     unstash 'html-files'
					
					sh '''

                    # Create container C1 if not exists
                    docker inspect C1 >/dev/null 2>&1 || docker run -dp 80:80 --name C1 httpd
					
					 docker exec C1 rm -f /usr/local/apache2/htdocs/index.html || true
					 
					 docker cp /mnt/workspace1/index.html C1:/usr/local/apache2/htdocs
					 
                    docker exec C1 chmod 644 /usr/local/apache2/htdocs/index.html
					 
					 
					
                    '''
				  
				  }
            }
        }
		
		
		
    }
}
