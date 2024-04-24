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
  def version = '1.0_' + env.BUILD_NUMBER
  node(POD_LABEL) {
    
    stage('Build Docker image') {
      git url:'https://github.com/HungNguyen0731/demo.git', branch: 'new'
      container('docker') {
        sh "docker build -t demoapp:${version} -f 'DEMO/Dockerfile' ."
      }
    }
      stage('Push image DockerHub') {
      container('docker') {
        sh "docker login -u hungnv31007 -p ${env.DOCKER_CI_PASSWORD} "
        sh "docker image tag demoapp:${version} hungnv/demoapp:${version}"
        sh "docker push hungnv/demoapp:123"
      }
    }
    
  }
}
