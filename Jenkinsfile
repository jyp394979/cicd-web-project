pipeline {
    agent any
    tools {
        jdk 'JDK11'
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
        
        stage('SonarQube analysis') {
            steps {
                withSonarQubeEnv('sonarqube-server') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
        
        stage('war deploy & build docker image') {
            steps {
                sshPublisher(publishers: [sshPublisherDesc(configName: 'docker ssh', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'docker build --tag=jyp394979/cicd-project-final -f Dockerfile . ;', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '.', remoteDirectorySDF: false, removePrefix: 'target', sourceFiles: 'target/*.war')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
            }
        }
        
        stage('run container') {
            steps {
                sshPublisher(publishers: [sshPublisherDesc(configName: 'ansible ssh', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'ansible-playbook -i hosts create-cicd-devops-container.yml;', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
            }
        }
    }
}