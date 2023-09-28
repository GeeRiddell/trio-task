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
                    docker build -t seethatgee/nginx:latest ./nginx
                    docker build -t seethatgee/flask-app:latest ./flask-app
                    docker build -t seethatgee/mysql:latest ./db
                    '''
                }
            }
            stage('push images'){
                steps{
                    sh '''
                    docker push seethatgee/nginx
                    docker push seethatgee/flask-app 
                    docker push seethatgee/mysql
                    '''
                }
            }
            stage('On Jenkins2 + run containers'){
                steps{
                    sh '''
                    ssh -i "~/.ssh/id_rsa" jenkins@10.200.0.19 << EOF
                    hostname
                    
                    docker pull seethatgee/flask-app
                    docker pull seethatgee/mysql
                    docker pull seethatgee/nginx
                    docker network inspect trioj && sleep 1 || docker network create trioj
                    docker run -d -p 80:80 --network=trioj --name nginx seethatgee/nginx
                    docker run -d --network=trioj --name flask-app seethatgee/flask-app
                    docker run -d --network=trioj --name mysql seethatgee/mysql
                    '''
                }
            }
            stage('Cleanup'){
                steps{
                    sh '''
                    docker rmi seethatgee/flask-app
                    docker rmi seethatgee/mysql
                    docker rmi seethatgee/nginx
                    '''
                }
            }
        }
}