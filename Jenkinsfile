pipeline {
    agent {
        node {
            label 'built-in'
        }
    }
    stages {
        stage('Continuous download') {
            step {
                git branch: 'main', credentialsId: 'classicstan', url: 'https://github.com/classicstan/School-webapp.git'
            }
        }
        stage ('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'sonarqube';
                    withSonarQubeEnv('sonarqube') {
                        sh "${scannerHome}/bin/sonar-scanner \
                            -D sonar. login=admin \
                            -D sonar. password Operations23 \
                            -D sonar.projectKey=School-23 \
                            -D sonar.exclusions=vendor/**, resources/**,**/*.java \
                            -D sonar.sources=/var/lib/jenkins/workspace/scripted/sre \
                            -D sonar.host.urlshttp://18.208.248.57:9000"
                    
                    }
                }
            }
        }
        stage('Continuous build') {
            steps sh {
                'mvn clean package'
            }
        }
        stage('Continuous testing') {
            steps sh {
                echo 'testing was successful'        
            }
        }
        stage('continous deployment') {
            step sh {
                deploy adapters: [tomcat9(credentialsid: 'dev', path: '', url: 'http://172.31.94.205:8080')], contextPath: 'QaEnv', war: '**/*.war'
            }
        }
        stage('continous dlivery') {
            steps {
                input message: 'are you authorized to run this job?', submitter: 'stanley'
                deploy adapters:[tomcat9(credentialsid: 'prod', path: '', url: 'http://172.31.93.91:8080')], contextPath: 'ProdEnv', war: '**/*.war' 
            }
        }    
    } 
}

