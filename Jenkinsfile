podTemplate(label: 'test', containers: [
  containerTemplate(name: 'jnlp', image: 'jenkins/jnlp-slave:latest', args: '${computer.jnlpmac} ${computer.name}'),
  containerTemplate(name: 'maven', image: 'maven:3.5.0-jdk-8', ttyEnabled: true, command: 'cat')
  ],
  volumes: [
    hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock'),
]) {
    node('test') {
      container('maven') {
        stage('Checkout') {
          checkout scm
        }
        stage('Build') {
          sh "mvn clean install -DskipTests"
        }

        stage('Unit Test') {
          sh "mvn test"
        }

        stage('Integration Test') {
          sh "mvn verify -DskipUnitTests -Parq-wildfly-swarm"
        }
      }
    }
}
