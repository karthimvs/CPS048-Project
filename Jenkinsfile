pipeline {
    agent {
        label 'primary'
    }
     stages {
        stage ('SCM Checkout') {
            steps {
                git credentialsId: 'cps048-git-cred', url: 'https://github.com/karthimvs/CPS048-Project.git'
            }
        }	

     }

}







