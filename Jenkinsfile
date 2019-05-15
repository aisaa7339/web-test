node('linux'){
    stage('Setup'){
        git credentialsId: 'HW11_docker', url: 'https://github.com/UST-SEIS665/hw11-seis665-03-spring2019-aisaa7339.git'
         sh 'aws s3 cp s3://seis665-public-assignment6/classweb.html index.html'
    }
    
    stage ('Build'){
         sh 'docker image build -t classweb:1.0 .'
    }   
    
    stage('Test'){
        sh 'docker stop classweb1 || true'
        sh 'docker rm classweb1 || true'
        
        sh 'docker run -d --name classweb1 -p 80:80 --env NGINX_PORT=80 classweb:1.0'
        sh 'curl -s $(docker inspect --format="{{.NetworkSettings.IPAddress}}" classweb1)'
                 
    }
}
