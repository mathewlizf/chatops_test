pipeline {
    agent {
      kubernetes {
        yaml """
    apiVersion: v1
    kind: Pod
    spec:
      containers:
      - name: jnlp
        image: m.daocloud.io/docker.io/jenkins/inbound-agent:latest
        imagePullPolicy: IfNotPresent
      - name: docker
        image: m.daocloud.io/docker.io/library/docker:24-cli
        imagePullPolicy: IfNotPresent
        command: ['cat']
        tty: true
        volumeMounts:
        - name: docker-sock
          mountPath: /var/run/docker.sock
      - name: kubectl
        image: m.daocloud.io/docker.io/bitnami/kubectl:latest
        imagePullPolicy: IfNotPresent
        command: ['cat']
        tty: true
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
                container('docker') {
                    sh "docker build -t chatops-test:latest ."
                }
            }
        }
        stage('Deploy to K8s') {
            steps {
                container('kubectl') {
                    sh "kubectl apply -f k8s/deployment.yaml"
                    sh "kubectl apply -f k8s/service.yaml"
                }
            }
        }
    }
}