pipeline {
    agent {
        docker { 
            image 'node:8' 
            label 'my-defined-label'
            args  '-v /tmp:/tmp'
        }
    }
    stages {
        stage('Test0') {
            steps {
                sh 'node --version'
            }
        }
       stage('Checkout'){

          checkout scm
       }

       stage('Test'){

         env.NODE_ENV = "test"

         print "Environment will be : ${env.NODE_ENV}"

         sh 'node -v'
         sh 'npm prune'
         sh 'npm install'
         sh 'npm test'

       }

       stage('Build Docker'){

            sh './dockerBuild.sh'
       }

       stage('Deploy'){

         echo 'Push to Repo'
         sh './dockerPushToRepo.sh'

         echo 'ssh to web server and tell it to pull new image'
         sh 'ssh deploy@xxxxx.xxxxx.com running/xxxxxxx/dockerRun.sh'

       }

       stage('Cleanup'){

         echo 'prune and cleanup'
         sh 'npm prune'
         sh 'rm node_modules -rf'

         mail body: 'project build successful',
                     from: 'xxxx@yyyyy.com',
                     replyTo: 'xxxx@yyyy.com',
                     subject: 'project build successful',
                     to: 'yyyyy@yyyy.com'
       }
    }
}
