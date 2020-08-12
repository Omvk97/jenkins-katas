pipeline {
  agent any
  environment {
    docker_username = 'omvk97'
  }
  stages {
    stage('Clone Down') {
      agent any
      steps {
        stash excludes: '.git', name: 'code'
      }
    }
    stage('Parallel execution') {
      parallel {
        stage('Say Hello') {
          steps {
            sh 'echo "hello world"'
          }
        }

        stage('Build App') {
          agent {
            docker {
              image 'gradle:jdk11'
            }
          }
          options {
            skipDefaultCheckout()
          }
          steps {
            unstash 'code'
            sh 'ci/build-app.sh'
            archiveArtifacts(artifacts: 'app/build/libs/', onlyIfSuccessful: true)
            stash excludes: '.git', name: 'code'
          }
        }

        stage('Test App') {
          agent {
            docker {
              image 'gradle:jdk11'
            }
          }
          options {
            skipDefaultCheckout()
          }
          steps {
            unstash 'code'
            sh 'ci/unit-test-app.sh'
            junit 'app/build/test-results/test/TEST-*.xml'
          }
        }
      }
    }
    stage('Push Docker App') {
      agent any
      environment {
        DOCKERCREDS = credentials('docker_login') //use the credentials just created in this stage
      }
      steps {
        unstash 'code' //unstash the repository code
        sh 'ci/build-docker.sh'
        sh 'echo "$DOCKERCREDS_PSW" | docker login -u "$DOCKERCREDS_USR" --password-stdin' //login to docker hub with the credentials above
        input 'Push image to DockerHub?'
        sh 'ci/push-docker.sh'
      }
    }
  }
  post {
    always {
      deleteDir() /* clean up our workspace */
    }
  }
}
