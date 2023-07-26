pipeline {
    agent any
        stages {
            stage('Build Maven'){
                steps {
                    sh 'pwd'
                    sh 'mvn clean install package'
                }
            }
            stage('Copy Artifact'){
                steps {
                    sh 'pwd'
                    sh 'cp -r target/*.jar docker'
                }
            }
            stage('Run Tests'){
                steps {
                    sh 'mvn test'
                }
            }
            stage('Build docker image'){
                steps {
                    script {
                        def customImage = docker.build("abhi3490/petclinic:${env.BUILD_NUMBER}", "./docker")
                        docker.withRegistry('https://hub.docker.com', 'docker hub'){
                        customImage.push()
                        }
                    }
                }
            }
        }
}
