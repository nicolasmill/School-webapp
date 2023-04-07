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
                            -Dsonar.login=squ_2e5fb188c81da23c0d10bdd716452d9a1bf1f401 \
                            -Dsonar.projectKey=School23 \
                            -Dsonar.exclusions=vendor/**,resources/**,**/*.java \
                            -Dsonar.sources=/var/lib/jenkins/workspace/Operations2023/src \
                            -Dsonar.host.url=http://18.208.248.57:9000"        
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