pipeline{
        agent any
        stages{
            stage('On Jenkins Server'){
                steps{
                    sh '''
                    echo "Hello, you are in jenkins server"
                    hostname
                    

                    '''
                }
            }
            stage('Build docker images'){
                steps{
                    sh '''
                    docker build -t nginx ./nginx
                    docker build -t flask-app ./flask-app
                    docker build -t mysql ./mysql
                    '''
                }
            }
            stage('On Jenkins2 + run containers'){
                steps{
                    sh '''
                    ssh -i "~/.ssh/id_rsa" jenkins@10.200.0.19 << EOF
                    hostname
                    docker network create trioj
                    docker run -d -p 80:80 --network=trioj seethatgee/nginx
                    docker run -d --network=trioj seethatgee/flask-app
                    docker run -d --network=trioj seethatgee/mysql
                    '''
                }
            }
        }
}