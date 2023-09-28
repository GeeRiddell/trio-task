pipeline{
        agent any
        stages{
            stage('Build docker images'){
                steps{
                    sh '''
                    docker build -t seethatgee/nginx:latest ./nginx
                    docker build -t seethatgee/flask-app:latest ./flask-app
                    docker build -t seethatgee/mysql:latest ./db
                    docker build -t seethatgee/nginx:${BUILD_NUMBER} ./nginx
                    docker build -t seethatgee/flask-app:${BUILD_NUMBER} ./flask-app
                    docker build -t seethatgee/mysql:${BUILD_NUMBER} ./db
                    '''
                }
            }
            stage('push images'){
                steps{
                    sh '''
                    docker push seethatgee/nginx
                    docker push seethatgee/flask-app 
                    docker push seethatgee/mysql
                    docker push seethatgee/nginx:${BUILD_NUMBER}
                    docker push seethatgee/flask-app:${BUILD_NUMBER} 
                    docker push seethatgee/mysql:${BUILD_NUMBER}
                    '''
                }
            }
            stage('On Jenkins2 + run containers'){
                steps{
                    sh '''
                    ssh -i "~/.ssh/id_rsa" jenkins@gemma-jenkins2 << EOF
                    docker stop nginx
                    docker stop mysql
                    docker stop flask-app
                    docker rm nginx
                    docker rm mysql
                    docker rm flask-app
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