    // Assuming yaml is same for all nodes - if not it can be passed as parameter
    podYaml= """
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

    pipeline {
        agent none
        stages {
            stage('Create List of Stages to run in Parallel') {
                steps {
                    script {
                        def map = ["name" : "demoapp1",
                                "name2" : "demoapp2"]
                        parallel map.collectEntries {
                            ["${it.key}" : generateStage(it)]
                        }
                    }
                }
            }
        }
    }

    def generateStage(job) {
        return {
            stage(job.key) {
                podTemplate(yaml:podYaml)  {
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
                            sh "docker image tag demoapp:${version} hungnv31007/demoapp:${version}"
                            sh "docker push hungnv31007/demoapp:${version}"
                        }
                        }
                    }
                }
            }
        }
    }
