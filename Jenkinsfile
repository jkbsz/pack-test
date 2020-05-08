pipeline {
   agent any

   tools {
      maven "M3"
   }

   stages {

      stage('Build') {
         steps {
             cleanWs()
            git 'https://github.com/jkbsz/pack-test.git'
            //git credentialsId: 'xxxxxxxxxxxxxxxxx', url: 'https://github.com/jkbsz/pack-test.git'
            sh "mvn -Dmaven.test.failure.ignore=true clean package deploy"
         }
         post {
            success {
               archiveArtifacts 'target/staticContent-*.zip'
            }
         }
      }

      stage('Input') {
          steps {
            input message: 'Release?', ok: 'Yes'
          }
      }

      stage('Release') {
         steps {
            sh 'mvn -B release:prepare release:perform'
         }
         post {
            success {
               archiveArtifacts 'target/staticContent-*.zip'
            }
         }
      }

   }
}