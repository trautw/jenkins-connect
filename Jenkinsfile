pipeline {
    agent {
        docker { 
            image 'node:8' 
            args  '-v /tmp:/tmp'
        }
    }
    environment {
        // stupid NPM configstore package!
        XDG_CONFIG_HOME = '.configstore'
        // stupid NPM username package!
        MYUSER = 'dummyuser'
       npm_config_cache = 'npm-cache'
       HOME = '.'
    }
    stages {
        stage('Example Build') {
            agent { docker 'maven:3-alpine' } 
            steps {
                echo 'Hello, Maven'
                sh 'mvn --version'
            }
        }
        
        stage('Example Test') {
            agent { docker 'openjdk:8-jre' } 
            steps {
                echo 'Hello, JDK'
                sh 'java -version'
            }
        }

        stage('Test0') {
            steps {
                sh 'node --version'
                sh 'ls -la'
                sh 'pwd'
                sh 'set'
                sh 'id'
            }
        }
/*
       stage('Checkout'){
            steps {
          checkout scm
            }
       }
*/
       stage('Test'){
            steps {

         sh 'export NODE_ENV="test"'

         print "Environment will be : ${env.NODE_ENV}"

         sh 'node -v'
         sh 'npm prune'
         sh 'npm install'
         sh 'npm test'

            }
       }

/*
       stage('Build Docker'){
            steps {

            sh './dockerBuild.sh'
            }
       }

       stage('Deploy'){
            steps {

         echo 'Push to Repo'
         sh './dockerPushToRepo.sh'

         echo 'ssh to web server and tell it to pull new image'
         sh 'ssh deploy@xxxxx.xxxxx.com running/xxxxxxx/dockerRun.sh'
            }

       }
*/

       stage('Cleanup'){
            steps {

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
    post {
      always {
        junit 'junit-results.xml'
      }
    }
}
