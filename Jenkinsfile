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
            stage('On Jenkins2'){
                steps{
                    sh '''
                    ssh -i "~/.ssh/id_rsa" jenkins@10.200.0.19 << EOF
                    echo "It's me, hi, I'm the problem it's me"
                    hostname
                    '''
                }
            }
        }
}