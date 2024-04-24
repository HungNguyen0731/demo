podTemplate(
    label: "kubernetes",
    containers: [
        containerTemplate(
            name: 'docker', 
            image: 'docker:latest', 
            ttyEnabled: true, 
            command: 'cat',
            volumeMounts: [
                mountPath: '/var/run/docker.sock',
                name: 'docker-socket'
            ]
        )
    ],
    volumes: [
        hostPathVolume(
            hostPath: '/var/run/docker.sock',
            name: 'docker-socket'
        )
    ]
) {
    node("kubernetes") {
        stage('Get a Golang project') {
            container('docker') {
                stage('Docker Build') {
                    sh "docker version"
                }
            }
        }
    }
}
