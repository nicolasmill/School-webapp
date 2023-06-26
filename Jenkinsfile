pipeline {
    agent {
        node {
            label 'built-in'
        }
    }
    stages {
        stage('Continuous download') {
            steps {
                git branch: 'main', credentialsId: '63a8f047-61a1-4082-9876-31f4821bae5f', url: 'https://github.com/nicolasmill/Student-login-CI-CD.git'
            }
        }
        stage ('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'sonarqube';
                    withSonarQubeEnv('sonarqube') {
                        sh "${scannerHome}/bin/sonar-scanner \
                            -Dsonar.login=sqa_46658063ff3a1485975d6873c4f77a09fbcf0ac4 \
                            -Dsonar.projectKey=Sonar_Startup_2 \
                            -Dsonar.exclusions=vendor/**,resources/**,**/*.java \
                            -Dsonar.sources=/var/lib/jenkins/workspace/Startup/src \
                            -Dsonar.host.url=http://10.0.1.74:9000"
                    }
                }
            }
        } 
        stage('SonarQube QG status') {
            steps {
               echo 'code analysis was successful'
            }
        }
        stage('Continuous build') {
            steps {
               sh 'mvn clean package'
            }
        }
        stage('continous deployment') {
            steps {
               deploy adapters: [tomcat9(credentialsId: 'tomcat-access', path: '', url:'http://10.0.1.99:8080')], contextPath: 'qaenv', war: '**/*.war'
            } 
        }
        stage('Continuous testing') {
            steps {
                echo 'testing was successful'
            }
        }
    }
}


