pipeline{
    agent {
        label 'AGENT-1'
    }
    options{
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds()
        ansiColor('xterm')
    }
    environment{
        def appVersion = '' //variable declaration
    }
    stages{
        stage("read the version"){
            steps{ 
                script{
                def packageJson = readJSON file: 'package.json'
                appVersion = packageJson.appversion
                echo "application version: $appVersion"
                }
            }
        }
        stage("installing dependencies"){
            steps{ 
                sh """
                npm install
                ls -ltr
                echo $appVersion
                echo "application version: $appVersion"
                """
            }
        }
        stage('Build'){
            steps{
                sh """
                zip -q -r backend-${appVersion}.zip * -x Jenkinsfile -x backend-${appVersion}.zip
                ls -ltr
                """
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