pipeline {
    agent {
      kubernetes {
        yaml """
    apiVersion: v1
    kind: Pod
    spec:
      containers:
      - name: jnlp
        image: jenkins/inbound-agent:latest
        imagePullPolicy: IfNotPresent
      - name: docker
        image: docker:24-cli
        imagePullPolicy: IfNotPresent
        command: ['cat']
        tty: true
        volumeMounts:
        - name: docker-sock
          mountPath: /var/run/docker.sock
      volumes:
      - name: docker-sock
        hostPath:
          path: /var/run/docker.sock
    """
      }
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/mathewlizf/chatops_test.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh "docker build -t chatops-test:latest ."
            }
        }
        stage('Deploy to K8s') {
            steps {
                sh "kubectl apply -f k8s/deployment.yaml"
                sh "kubectl apply -f k8s/service.yaml"
            }
        }
    }
}