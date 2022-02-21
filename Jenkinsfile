pipeline {

  agent any

  stages {

    
      stage("Build image") {
            steps {
                script {
                    myapp = docker.build("androshchuk/hellowhale:${env.BUILD_ID}") /*Скрипт для сворення контейнера і параметри куди його запушити*/
                }
            }
        }
    
      stage("Push image") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') { /*Параметри для входу в докерхаб*/
                            myapp.push("latest")
                            myapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }

    
    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "app.yaml", kubeconfigId: "mykubeconfig") /*Скрипт для доставки на кластер вмісту, який описаний в app.yaml*/
        }
      }
    }

  }

}
