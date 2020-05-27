
pipeline {
   agent any

   stages {
      stage('checkout_Code_integration') {
         steps {
            checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'd3f28fd7-b805-4ab1-8f2c-6ad1bbe07479', url: 'https://github.com/vihaljain/CICD.git']]])

         }
      }
      stage('Unit_Testing') {
         steps {
                        sh "/usr/local/bin/pip install -r requirements.txt"
            sh "/usr/bin/python -m pytest -v tests/test_generator.py"
         }
      }
      stage('Docker_image_Build') {
         steps {

            sh "/usr/bin/docker build -t vihaljain/cicd-example:latest ."

         }
      }
      stage('publish') {
         steps {
			withDockerRegistry(credentialsId: 'd3f28fd7-b805-4ab1-8f2c-6ad1bbe07479', url: 'https://index.docker.io/v1/') {
                     sh "/usr/bin/docker push vihaljain/cicd-example:latest"
         }
         }
      }
      stage('Running_image_from_DockerHub') {
         steps {

           sh "/usr/bin/docker run -p 5000:5000 --rm vihaljain/cicd-example:latest"
         }
         }
      }
   }
