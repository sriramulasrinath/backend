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
        nexusUrl = "nexus.srinath.online:8081"
    }
    stages{
        stage("read the version"){
            steps{ 
                script{
                    def packageJson = readJSON file: 'package.json'
                    appVersion = packageJson.version
                    echo "application version: $appVersion"
                }
            }
        }
        stage("installing dependencies"){
            steps{ 
                sh """
                npm install
                ls -ltr
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
        stage('Sonar Scan') {
            environment {
                scannerHome = tool 'sonar-6.0' //--> which version i want mentioned here referring scanner CLI
            }
            steps {
                script {
                    withSonarQubeEnv('sonar-6.0') { // refering the server
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }
        stage('nexus artifactory uploading'){
            steps{
                script{
                    nexusArtifactUploader(
                        nexusVersion: 'nexus3',
                        protocol: 'http',
                        nexusUrl: "${nexusUrl}",
                        groupId: 'com.expense',
                        version: "${appVersion}",
                        repository: 'backend',
                        credentialsId: 'nexus-auth',
                        artifacts: [
                            [artifactId: "backend",
                            classifier: '',
                            file: "backend-${appVersion}.zip",
                            type: 'zip']
                        ]
                    )
                }
            }
        }
        // stage('Deploy'){
        //     steps{
        //         script{
        //             def params = [
        //                 string(name: 'appVersion', value: "${appVersion}")
        //             ]
        //             build job: 'backend-deploy', parameters: params, wait: false
        //         }
        //     }
        // }
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