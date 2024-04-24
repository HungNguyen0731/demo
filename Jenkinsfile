podTemplate(
    label:"kubernetes",
    containers: [
    containerTemplate(name: 'docker', image: 'docker:latest', ttyEnabled: true, command: 'cat')
  ]) {

    node("kubernetes") {
           stage('Get a Golang project') {
            git url: 'https://github.com/hashicorp/terraform.git'
            container('docker') {
                stage('Docker Build') {
                    sh "docker version"
                }
            }
        }

    }
}
