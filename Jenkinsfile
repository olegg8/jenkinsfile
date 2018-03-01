def mvn(def args) {
    //def mvnHome = tool 'M3'
    //def javaHome = tool 'JDK8'

    withEnv(["JAVA_HOME=${javaHome}", "PATH+MAVEN=${mvnHome}/bin:${env.JAVA_HOME}/bin"]) {
        sh "${mvnHome}/bin/mvn ${args} --batch-mode -V -U -e -Dsurefire.useFile=false"
    }
}

podTemplate(label: 'test', containers: [
  containerTemplate(name: 'jnlp', image: 'jenkins/jnlp-slave:latest', args: '${computer.jnlpmac} ${computer.name}'),
  containerTemplate(name: 'maven', image: 'maven:3.5.0-jdk-8', ttyEnabled: true, command: 'cat')
  ],
  volumes: [
  hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock'),
]) {
    node('test') {
      container('maven') {
        stage('Build') {
          mvn 'clean install -DskipTests'
        }

        stage('Unit Test') {
          mvn 'test'
        }

        stage('Integration Test') {
          mvn 'verify -DskipUnitTests -Parq-wildfly-swarm '
        }
      }
    }
}
