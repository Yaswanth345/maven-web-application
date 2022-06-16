pipeline{
    agent any
    
    triggers{
        pollSCM('* * * * *')
    }
    options{
        buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5'))
    }
    tools{
        maven "maven3.8.5"
    }
    stages{
        stage("CheckoutCode"){
            steps{
                git branch: 'development', url: 'https://github.com/Yaswanth345/maven-web-application.git'
            }
        }
        stage("Build"){
            steps{
                sh "mvn clean package"
            }
        }
        stage("UploadingBuildIntoNexus"){
            steps{
                sh "mvn clean deploy"
            }
        }
        stage("DeployIntoTomcat"){
            steps{
                 sshagent(['f5ae42d5-e780-4ef2-9697-7a715742cc2a']) {
                 sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@15.207.51.253:/opt/apache-tomcat-9.0.63/webapps/"
                }
            }
        }
    }
    post{
        
        success{
            emailext body: '''Build Is Successfull

            Regards
            Yaswanth''', subject: 'Build Successfull', to: 'yaswanthgopi918@gmail.com'
        }
        
        failure{
            emailext body: '''Build Is Failure

            Regards
            Yaswanth''', subject: 'Build Failure', to: 'yaswanthgopi918@gmail.com'
        }
    }
}
