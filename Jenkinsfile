node('linux'){
    stage('Test Stack'){
         git credentialsId: 'GITHUB', url: 'https://github.com/aisaa7339/web-test.git'
         withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'AWSCRENDENTIAL', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
         sh 'aws cloudformation create-stack --region us-east-1 --stack-name final-test --template-body file://docker-single-server.json --parameters ParameterKey=KeyName,ParameterValue=Exam ParameterKey=YourIp,ParameterValue=$(curl ifconfig.me)/32'
         sh 'aws cloudformation wait stack-create-complete --stack-name final-test --region us-east-1'
         sh 'aws cloudformation describe-stacks --stack-name final-test --region us-east-1'
         env.docker1IP = sh returnStdout: true, script: 'aws cloudformation describe-stacks --stack-name final-test --region us-east-1 --query Stacks[].Outputs[].[OutputValue] --output text'
         sshagent(['Exam_Practice']) {
              sh 'ssh -o StrictHostKeyChecking=no ubuntu@${docker1IP} uptime'
   
        }       
   
        }
         
    }
    
    stage('Deploy Redis Stanalone') {
        sshagent(['Exam_Practice']) {
                sh 'ssh ubuntu@${docker1IP} docker run -d --name redis -p 6379:6379 redis:latest'
        }
        
    }
    
    stage('Test Redis Standalone') {
        sshagent(['Exam_Practice']) {
            sh 'ssh ubuntu@${docker1IP} redis-cli set hello world'
            sh 'ssh ubuntu@${docker1IP} redis-cli get hello'
            
        }
        
    }
    
    stage('Delete Test Stack') {
        withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'AWSCRENDENTIAL', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
            sh 'aws cloudformation delete-stack --stack-name final-test --region us-east-1'
			sh 'aws cloudformation wait stack-delete-complete --stack-name final-test --region us-east-1'
            
        }
        
        
    }
}
    
