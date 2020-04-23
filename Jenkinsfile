def label =  "gsuite-extactor-${UUID.randomUUID().toString()}"
def k8sManifestName = "manifests.yaml"
def version

podTemplate(
    label: label,
    annotations: [
        podAnnotation(key: "jenkins/build-url", value: env.BUILD_URL ?: ""),
        podAnnotation(key: "jenkins/github-pr-url", value: env.CHANGE_URL ?: ""),
    ],
    containers: [
        containerTemplate(name: 'golang',
            image: 'ubuntu:latest',
            command: '/bin/cat -',
            resourceRequestCpu: '2000m',
            resourceRequestMemory: '2000Mi',
            resourceLimitCpu: '2000m',
            resourceLimitMemory: '2000Mi',
            ttyEnabled: true
        ),
        containerTemplate(
            name: 'docker',
            command: '/bin/cat -',
            image: 'docker:17.06.2-ce',
            resourceRequestCpu: '200m',
            resourceRequestMemory: '300Mi',
            resourceLimitCpu: '200m',
            resourceLimitMemory: '300Mi',
            ttyEnabled: true
        )
    ],
    volumes: [
        hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock')]) {
    properties([buildDiscarder(logRotator(daysToKeepStr: '30', numToKeepStr: '20'))])
    node(label) {
        stage('Checkout') {
            checkout(scm)
            imageRevision = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()
            buildDate = sh(returnStdout: true, script: 'date +%Y-%m-%dT%H%M').trim()
            dockerImageTag = "${buildDate}-${imageRevision}"
            version = sh(returnStdout: true, script: 'cat version.txt').trim()
        }

        stage('Build GCP metadata') {
            container('golang') {
                sh('echo "Hello world2"')
            }
        }
    }
}
