pipeline{
    agent any
    triggers {
        pollSCM '* * * * *'
        }
    stages{
        stage("build nmp"){
            steps{
                sh "nmp install"
            }
        }
        stage("build image"){
            steps{
                withCredentials([usernamePassword(credentialsId: 'docker-hub', passwordVariable: 'password', usernameVariable: 'username')]) {
                    sh "docker build -t rufeezy/$JOB_NAME:$BUILD_NUMBER ."
                    sh "docker login -u $username -p $password"
                    sh "docker push rufeezy/$JOB_NAME:$BUILD_NUMBER"
                }
            }
        }
        stage("deploy container"){
            steps{
                 withCredentials([usernamePassword(credentialsId: 'docker-hub', passwordVariable: 'password', usernameVariable: 'username')]) {
                sh "ansible-playbook -i inventory -u ansible playbook.yaml -e JOB_NAME=$JOB_NAME -e BUILD_NUMBER=$BUILD_NUMBER -e username=$username -e password=$password"
                }
             }
        }
    }
}
