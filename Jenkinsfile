pipeline {
  agent any
  stages {
    stage ('Build') {
      steps {
        sh 'printenv'
        sh 'docker build -t bigugo21/jenkinsdemo:""$GIT_COMMIT"" .'
      }
    }
    
     stage ('Publish to DockerHub') {
      steps {
        withDockerRegistry([credentialsId: "docker-hub", url: ""]) {
          sh 'docker push bigugo21/jenkinsdemo:""$GIT_COMMIT""'
         }
       }
     }
    stage ('Publish to ECR') {
      steps {
        //sh 'aws ecr-public get-login-password --region eu-west-2 | docker login --username AWS --password-stdin public.ecr.aws/f5e0m8x3'
        //withAWS(credentials: 'sam-jenkins-demo-credentials', region: 'us-west-2') {
         withEnv(["AWS_ACCESS_KEY_ID=${env.AWS_ACCESS_KEY_ID}", "AWS_SECRET_ACCESS_KEY=${env.AWS_SECRET_ACCESS_KEY}", "AWS_DEFAULT_REGION=${env.AWS_DEFAULT_REGION}"]) {
          sh 'docker login -u AWS -p $(aws ecr-public get-login-password --region us-east-1) public.ecr.aws/f5e0m8x3' 
          sh 'docker build -t ugoecr .'
          sh 'docker tag ugoecr:public.ecr.aws/f5e0m8x3/ugoecr:""$BUILD_ID""'
          sh 'docker push public.ecr.aws/f5e0m8x3/ugocothrepo:latest' ""$BUILD_ID""'
         }
       }
    }
  }
}
