pipeline {
    agent any
    tools {
        maven 'Maven'
    }
    stages {
        stage("Pull SRC") {
            steps {
                git 'https://github.com/Harshithvk/Instagram'
            }
        }
        stage("Prepare Build") {
            steps {
                sh 'mvn clean package'
            }
        }
        stage("Copy *.war file to ansible") {
            steps {
                sh 'mv target/reels.war .'
                sshPublisher(
                    continueOnError: false, 
                    failOnError: true,
                    publishers: [
                        sshPublisherDesc(
                            configName: "marcos",
                            transfers: [sshTransfer(sourceFiles: 'reels.war')],
                            verbose: true
                        )
                    ]
                )
            }
        }
        stage("Copy Docker file to ansible") {
            steps {
                sshPublisher(
                    continueOnError: false, 
                    failOnError: true,
                    publishers: [
                        sshPublisherDesc(
                            configName: "marcos",
                            transfers: [
                                sshTransfer(sourceFiles: 'Dockerfile'),
                                sshTransfer(execCommand: "docker rm -f tomcat; docker rmi marcos; docker build -t marcos .; docker run -it -d -p 8081:8080 --name tomcat marcos")
                            ],
                            verbose: true
                        )
                    ]
                )
            }
        }
    }
}
