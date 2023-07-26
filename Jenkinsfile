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
                        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub'){
                        customImage.push()
                        }
                    }
                }
            }
            stage('Build on kubenetes'){
                steps {
                    withKubeConfig([credentialsId: 'kubeconfig']){
                        sh 'pwd'
                        sh 'cp -r helm/* .'
                        sh 'ls -ltrh'
                        sh 'pwd'
                        sh '/usr/local/bin/helm upgrade --install petclinic-app petclinic --set image.repository=abhi3490/petclinic --set image.tag=$(BUILD_NUMBER)'
                    }
                }    
            }
        }
}
