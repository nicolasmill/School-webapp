pipeline {
    agent {
        node {
            label 'built-in'
        }
    }
    stages {
        stage('Continuous download') {
            steps {
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
            steps {
               sh 'mvn clean package'
            }
        }
        stage('Continuous testing') {
            steps {
                echo 'testing was successful'
            }
        }
        stage('continous deployment') {
            steps {
               deploy adapters: [tomcat9(credentialsId: '19c73ccc-154b-43db-9320-7d28507070c2', path: '', url: 'http://172.31.94.205:8080')], contextPath: 'qaenv', war: '**/*.war'
            }
        }
        stage('continous dlivery') {
            steps {
                input message: 'are you authorized to run this job?', submitter: 'stanley'
                deploy adapters: [tomcat9(credentialsId: 'Prodcredentials', path: '', url: 'http://172.31.93.91:8080')], contextPath: 'prodenv', war: '**/*.war'
            }
        }
        
        
    }
}

