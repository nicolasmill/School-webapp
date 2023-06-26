pipeline {
    agent {
        node {
            label 'built-in'
        }
    }
    stages {
        stage('Continuous download') {
            steps {
                git branch: 'main', credentialsId: 'nicolasmill', url: 'https://github.com/nicolasmill/Student-login-CI-CD.git'
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
               deploy adapters: [tomcat9(credentialsId: 'Dev', path: '', url:'http://3.87.252.234:8080')], contextPath: 'qaenv', war: '**/*.war'
            }
        }
        stage('Continuous testing') {
            steps {
                echo 'testing was successful'
            }
        }
        /*stage('deploy artifact') {
            steps {
                nexusArtifactUploader artifacts: [
                        [
                            artifactid: 'tt.test', 
                            classifier: '', 
                            file: '/var/lib/jenkins/workspace/Operations23/target/tt.test.war', 
                            type: 'war'
                        ]
                    ], 
                    credentialsid: 'nexus', 
                    groupld: 'com.tt', 
                    nexusUrl: '54.91.82.201:8081',
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: 'Operations23', 
                    version: '*1.0-SNAPSHOT'
                
            }
        }
        stage('continous dlivery') {
            steps {
                input message: 'are you authorized to run this job?', submitter: 'stanley' */
               // deploy adapters: [tomcat9(credentialsId: 'Prodcredentials', path: '', url: 'http://172.31.93.91:8080')], contextPath: 'prodenv', war: '**/*.war'
       //     }
      //  }
       /* stage( 'Post-Build Notification') {
            steps {
                script {
                    def status = currentBuild.currentResult
                    def message = "Build ${status}: Job '$ {env. JOB_NAME} [$ {env.BUILD_NUMBER}]'" 
                    slackSend (
                        channel: 'operation2023',
                        color: status == 'SUCCESS' ? 'good' : 'danger' ,
                        message: message, 
                        tokenCredentialId: 'slack'
                    )
                }
            }
        }    
    
    }
    post {
        always {
            script {
                slacksend (
                    channel: 'operation2023',
                    color: currentBuild.currentResult == 'SUCCESS' ? 'good' : 'danger',
                    message: "Build ${currentBuild.currentResult}: Job '$ {env.JOB_NAME} [${env.BUILD_NUMBER}]'"
                    message: "Build ${status}:Job '$ {env.JOB_NAME} [${env.BUILD_NUMBER}]'", 
                    tokenCredentialId: 'slack'
                )
            }
        }
    }
}
*/
