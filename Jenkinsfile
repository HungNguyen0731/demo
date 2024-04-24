podTemplate(yaml: """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: docker
    image: docker:latest
    command: ['cat']
    tty: true
    volumeMounts:
    - name: dockersock
      mountPath: /var/run/docker.sock
  volumes:
  - name: dockersock
    hostPath:
      path: /var/run/docker.sock
"""
  ) {

  def image = "jenkins/jnlp-slave"
  node(POD_LABEL) {
    stage('Build Docker image') {
      git url:'https://github.com/HungNguyen0731/demo.git', branch: 'new'
      container('docker') {
        sh "docker build -t demoapp:123 -f 'DEMO/Dockerfile' ."
      }
    }
  }
}
