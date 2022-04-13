pipeline {

  agent {
        kubernetes {
        }
  }
  stages {

    stage('Checkout Source') {
      steps {
        checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'joy-git', url: 'https://github.com/Joyc132/assesment.git']]])
      }
    }
    
      stage('dockerhub-login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'joy-doc', passwordVariable: 'pass', usernameVariable: 'uname')]) {
                sh 'docker login -u $uname -p $pass'
}
            }
        }
        stage('dockerimage-build') {
            steps {
                sh '''
                docker build -t my-image .
                docker tag my-image joy132/assesment:nginx-v2
                '''
            }
        }
        stage('dockerimage-push') {
            steps {
                sh 'docker push joy132/assesment:nginx-v2'
                
            }
        }
        stage('deployment') {
            steps{
                script {
                     sshPut remote: remote, from: "deployment.yml", into: "/home/joyc_chatterjee13/continuous-deployment-on-kubernetes"
                     sshCommand remote: remote, command: "kubectl apply -f deployment.yml"
                     sshCommand remote: remote, command: "kubectl get deployment"
                    }
                }
           }
        }
  }
