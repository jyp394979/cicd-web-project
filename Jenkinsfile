pipeline {
    agent any
    tools {
        maven 'maven3.9.9'
    }
    
    stages {
        stage('github clone') {
            steps {
                git branch: 'main', url: 'https://github.com/jyp394979/cicd-web-project.git'
            }
        }
        
        stage('build') {
            steps {
                sh '''
                    echo build start
                    mvn clean compile package -DskipTests=true
                '''
            }
        }
        
        stage('deploy') {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'deployer_user', path: '', url: 'http://localhost:8088')], contextPath: null, war: '**/*.war'
            }
        }

    }
}
