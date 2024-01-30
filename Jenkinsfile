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
                }
            }
        }
        stage('SonarQube Analysis') {
            steps {
                script {
                    def mvnHome = tool name: 'maven3.9.6', type: 'maven'
                    withSonarQubeEnv('sonar') {
                        sh "${mvnHome}/bin/mvn clean verify sonar:sonar \
                        -Dsonar.projectKey=cps048-project \
                        -Dsonar.projectName='cps048-project' \
                        -Dsonar.host.url=http://54.158.119.56:9000 \
                        -Dsonar.token=sqp_3e0d83dbbd79923b981b386ce898f6d8bfbbc257"

                    }
                }
            }
        }
        stage('Build Docker Image'){
            steps{
                sh 'sudo docker build -t cps048:v1 .'
            }
        }
        stage('Docker Deployment'){
            steps {
                sh 'sudo docker run -itd -p "7000:8080" --name tomcate-cps048 cps048:v1'
            }
        }

     }

}







