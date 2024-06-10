pipeline{
    agent {
        label 'AGENT-1'
    }
    options{
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds()
        ansiColor('xterm')
    }
    
    stages{
        stage("Build"){
            steps{
                sh 'echo this is build'
            }
        }
        stage("test"){
            steps{
                sh 'echo this is test'
                'echo ls -ltr'
            }
        }
        stage("Deploy"){
            steps{
                sh 'echo this is deploy'
            }
        }
    }
    post { 
        always { 
            echo 'I will always say Hello again!'
            deleteDir()
        }
        success { 
            echo 'I will when pipeline is success!'
        }
        failure { 
            echo 'I will when pipeline is failure!'
        }
    }
}