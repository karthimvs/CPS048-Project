pipeline {
    agent {
        label 'primary'
    }
     stages {
        stage ('Git Pull') {
            steps {
                git credentialsId: 'cps048-git-cred', url: 'https://github.com/karthimvs/CPS048-Project.git'
            }
        }
         stage ('Compile Maven') {
            steps {
                script {
                    def mvnHome = tool name: 'maven3.9.6', type: 'maven'
                    sh "${mvnHome}/bin/mvn clean package"
                    sh 'mv target/myweb*.war target/app1.war'
                }
            }
        }
        stage('SonarQube Analysis') {
            steps {
                script {
                    def mvnHome = tool name: 'maven3.9.6', type: 'maven'
                    withSonarQubeEnv('sonar') {
                        sh "${mvnHome}/bin/mvn sonar:sonar \
                        -Dsonar.projectKey=cps048-project \
                        -Dsonar.projectName='cps048-project" 
                    }
                }
            }
        }	

     }

}







