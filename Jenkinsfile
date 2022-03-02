pipeline {
    agent any
    stages {
/*       stage('Setup parameters') {
           steps {
               script {properties([parameters([choice(choices: 'master\nfeature', description: 'select the branch to build ', name: 'branch')])])}
           }
       }
*/
       stage('Validate') {
            steps {
                echo 'Validate Code'
                sh 'mvn compile'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
                sh 'mvn test'
            }
        }
        stage('Package') {
            steps {
                echo 'Packaging....'
                sh 'mvn package'
            }
        }
/*        
        stage('Copy war') {
            steps {
                echo 'copying ...'
                sh 'sudo cp /var/lib/jenkins/workspace/project/target/*.war /home/centos/'
                echo 'copied'
            }
       }

*/         
    
    
     stage('Build and push Docker images..') {
     environment {
            DOCKERHUB_CREDENTIALS=credentials('dockerjenkins')
        }
      steps{
       sh "sudo docker image build -t $JOB_NAME:v1.$BUILD_ID /var/lib/jenkins/workspace/project/."
       sh "sudo docker image tag $JOB_NAME:v1.$BUILD_ID phani09/$JOB_NAME:v1.$BUILD_ID"
       sh "sudo docker image tag $JOB_NAME:v1.$BUILD_ID phani09/$JOB_NAME:latest"
       //sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
       sh "sudo docker image push phani09/$JOB_NAME:v1.$BUILD_ID"
       sh "sudo docker image push phani09/$JOB_NAME:latest"
       sh "sudo docker image rmi $JOB_NAME:v1.$BUILD_ID phani09/$JOB_NAME:v1.$BUILD_ID phani09/$JOB_NAME:latest"
        
      }
  
  }
    
        stage('Run ansible'){
            steps{
                sh 'ssh -t -t root@ip-172-31-12-130.us-east-2.compute.internal'
                sh 'ssh-keygen -R 172.31.12.130'
                sh 'ansible-playbook depl.yml'
            }
        }
/*        
    stage('Deploy to K8S') {
      steps{
       sh 'kubectl apply -f jenk.yml'
           }
           }
*/           
    }
}


