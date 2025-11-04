pipeline {
    agent { label 'devops1-agent' }

    stages {
        stage('Pull SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/mubinibum/simple-apps.git'
            }
        }
        
        stage('Build') {
            steps {
                sh'''
                cd app
                npm install
                '''
            }
        }
        
        stage('Testing') {
            steps {
                sh'''
                cd app
                npm test
                npm run test:coverage
                '''
            }
        }
        
        stage('Code Review') {
            steps {
                sh'''
                cd app
                sonar-scanner \
                    -Dsonar.projectKey=simple-apps \
                    -Dsonar.sources=. \
                    -Dsonar.host.url=http://172.23.3.116:9000 \
                    -Dsonar.login=sqp_f161a85dc9716621fe663b7c6d1be999c5c9ac14
                '''
            }
        }
        
        stage('Deploy') {
            steps {
                sh'''
                docker compose up --build -d
                '''
            }
        }
        
        stage('Backup') {
            steps {
                 sh 'docker compose push' 
            }
        }
    }
}
