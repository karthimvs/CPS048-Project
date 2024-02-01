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
                        -Dsonar.host.url=http://3.88.4.83:9000 \
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
        stage ('Remove Existing Container') {
            steps {
                sh 'sudo docker rm -f tomcate-cps048'
            }
        }
        stage('Docker Deployment'){
            steps {
                sh 'sudo docker run -itd -p "7000:8080" --name tomcate-cps048 cps048:v1'
            }
        }
        stage('Nexus Image Push'){
            steps{
                sh " echo "admin@123" | docker login -u admin --password-stdin http://3.84.245.187:8085"
                sh "docker tag cps048:v1  3.84.245.187:8081/cps048:v1"
                sh 'docker push 3.84.245.187:8081/cps048:v1'
            }
   }

     }

}







