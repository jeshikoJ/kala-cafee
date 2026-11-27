pipeline {
  agent any
  triggers { pollSCM('* * * * *') }
  stages {
    stage('Deploy') {
      steps {
        sshagent(['azure-vm-key']) {
          sh '''
            rsync -avz --delete -e "ssh -o StrictHostKeyChecking=no" \
              --exclude .git ./ azureuser@172.198.136.153:~/kala-cafe/
            ssh -o StrictHostKeyChecking=no azureuser@172.198.136.153 "
              cd ~/kala-cafe &&
              docker build -t kala-cafe . &&
              (docker rm -f kala-cafe || true) &&
              docker run -d --name kala-cafe --restart unless-stopped -p 127.0.0.1:8081:80 kala-cafe"
          '''
        }
      }
    }
  }
}
