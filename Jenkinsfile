pipeline {
  agent {
    label 'jdk8'
  }
  stages {
    stage('Say Hello') {
      steps {
        echo "Hello ${params.Name}!"
        sh 'java -version'
        echo "${TEST_USER_USR}"
        echo "${TEST_USER_PSW}"
      }
    }
 //   stage('Deploy') {
   //   options {
   //     timeout(time: 30, unit: 'SECONDS')
  //    }
  //    input {
  //      message 'Which version?'
  //      ok 'deploy'
  //      parameters {
  //          choice(name: 'APP_VERSION', choices: "v1.1\nv1.2\nv1.3", description: 'What to deploy?')
  //      }
  //    }
  //    steps {
  //      echo 'Continuing with deployment'
 //     }
 //   }
     stage('Get Kernel') {
      steps {
        script {
          try {
            KERNEL_VERSION = sh (script: "uname -r", returnStdout: true)
          } catch(err) {
            echo "CAUGHT ERROR: ${err}"
            throw err
          }
        }
      }
    }
    stage('Say Kernel') {
      steps {
        echo "${KERNEL_VERSION}"
      }
    }
     stage('Testing') {
        parallel {
          stage('Java 8') {
            agent { label 'jdk9' }
            steps {
              container('maven8') {
                sh 'mvn -v'
              }
            }
          }
          stage('Java 9') {
            agent { label 'jdk8' }
            steps {
              container('maven9') {
                sh 'mvn -v'
              }
            }
          }
        }
      }

    stage('Checkpoint') {
         agent none
         steps {
            checkpoint 'Checkpoint'
         }
      }
    
    
  }
  post {
    aborted {
      echo 'Why didn\'t you push my button?'
    }
  }
  environment {
    MY_NAME = 'Mary'
    TEST_USER = credentials('test-user')
  }
 // parameters {
   // string(name: 'Name', defaultValue: 'whoever you are', description: 'Who should I say hi to?')
 // }
}
