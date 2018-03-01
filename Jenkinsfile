podTemplate(label: 'test', containers: [
  containerTemplate(name: 'jnlp', image: 'jenkins/jnlp-slave:latest', args: '${computer.jnlpmac} ${computer.name}'),
  containerTemplate(name: 'maven', image: 'maven:3.5.0-jdk-8', ttyEnabled: true, command: 'cat')
  ],
  volumes: [
    hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock'),
]) {
    node('test') {

      git branch: '3-declarative',
          credentialsId: '3d11bf3a-974e-46e9-9bf9-872734a65798',
          url: 'git@github.com:AlexandrSemak/jenkinsfile.git'

      container('maven') {
        //stage('Checkout') {
          //checkout scm
        //}
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
