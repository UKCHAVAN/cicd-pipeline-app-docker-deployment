pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }//build stage
        stage('build docker image'){
            when{
                 branch 'master'
              }//when
            steps{
                script{
                     app = docker.build("dockerbox2/train-schedule")
                    app.inside{
                      sh 'echo $(curl localhost:8080)'
                    }//inside
                }//script
            }//steps
         }//build dockerimage
        stage('Push Docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_login') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }//publish docker image
        
    }//stages
}//pipeline
