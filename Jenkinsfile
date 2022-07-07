pipeline{
    agent any
    triggers {
        pollSCM '* * * * *'
        }
    stages{
        stage("build"){
            steps{
                sh "docker build -t rufeezy/$JOB_NAME:$BUILD_NUMBER ."
            }
        }
        stage("deploy image to registry"){
            steps{
                withCredentials([usernamePassword(credentialsId: 'docker-hub', passwordVariable: 'password', usernameVariable: 'username')]) {
                    sh "docker login -u $username -p $password"
                    sh "docker push rufeezy/$JOB_NAME:$BUILD_NUMBER"
                }
            }
        }
        stage("deploy container"){
            steps{	    
                sh "docker run -d -p 80$BUILD_NUMBER:3000 rufeezy/$JOB_NAME:$BUILD_NUMBER"
         }
       }
    }
}
