
pipeline {
   agent any

   stages {
      stage('checkout_Code_integration') {
         steps {
            checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '65adad77-7981-400f-922b-74cc6ba0c88e', url: 'https://github.com/chaitanyagaajula/CICD.git']]])

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

            sh "/usr/bin/docker build -t chaitanyagaajula/cicd-example:latest ."

         }
      }
      stage('publish') {
         steps {
			withDockerRegistry(credentialsId: '65adad77-7981-400f-922b-74cc6ba0c88e', url: 'https://index.docker.io/v1/') {
                     sh "/usr/bin/docker push chaitanyagaajula/cicd-example:latest"
         }
         }
      }
      stage('Running_image_from_DockerHub') {
         steps {

           sh "/usr/bin/docker run -p 5000:5000 --rm chaitanyagaajula/cicd-example:latest"
         }
         }
      }
   }
