pipeline {
    agent {
        docker {
            image 'maven:3-alpine'
            args '-v /root/.m2:/root/.m2'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
        }
    }
}
rtServer (
   id: "Artifactory",
   url: "http://localhost:8081/artifactory"
   username: "admin",
   password: "iman1996"
   bypassProxy: true
   timeout = 300
)
rtUpload (
   serverId: "Artifactory",
   spec:
       """{
         "files": [
           {
             "pattern": "/var/lib/jenkins/workspace/mavenpro/target",
             "target": "example-repo-local/gradle-build-scan-quickstart/unspecified/"
           }
        ]
       }"""
)
