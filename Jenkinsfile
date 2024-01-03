pipeline {

  agent any

  options {

    buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')

  }
stages {
        
        stage('Cleanup Workspace') {
            steps {
                cleanWs()
                sh """
                echo "Cleaned Up Workspace For Project"
                """
            }
        }

        stage('Code Checkout') {
            steps {
                checkout([
                    $class: 'GitSCM', 
                    branches: [[name: '*/main']], 
                    userRemoteConfigs: [[url: 'https://github.com/erik-aerdts/multibranch.git']]
                ])
            }
        }

        stage(' Unit Testing') {
            steps {
                sh """
                echo "Running Unit Tests"
                """
            }
        }


        stage('Getting source') {
          steps {
           echo 'Getting source..'

                git branch: 'main',
                  url: 'https://github.com/erik-aerdts/multibranch.git'
               }
        }

        stage('Deploy code DEV server ') {
            steps {
                sh """
                echo "Running Code Analysis"
                """

            }
        }

        stage('Build Deploy Code on 172.17.1.22') {
 

        steps {

                withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId:'jenkins', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
                sh """
                echo "Deploying Code"
                """

           script {
         
            def remote = [:];
            remote.name = "testserver";

            remote.host = "172.17.1.22";

            remote.allowAnyHosts = true;
            remote.user = USERNAME;
            remote.password = PASSWORD;
            
            sshCommand remote: remote, command: "cp /var/www/html/index.html /var/www/html/index.old -f"
            sshPut remote:remote, from: "index.html", into:'/var/www/html/'
            
                   }

                                             } 
                    }
            }
        

       
}

}
