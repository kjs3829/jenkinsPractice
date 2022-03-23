pipelin {
    // 스테이지 별로 다른 거
    agent any

    triggers {
        pollSCM('*/3 * * * *')
    }

    environment {
        AWS_ACCESS_KEY_ID = credentials('awsAccessKeyId')
        AWS_SECRET_ACCESS_KEY = credentials('awsSecretAccessKey')
        AWS_DEFAULT_REGION = 'ap-northeast-2'
        HOME = '.' // Avoid npm root owned
    }

    stages {
        // 레포지토리를 다운로드 받음
        stage('Prepare') {
            agent any

            steps {
                echo 'Clonning Repository'

                git url: 'hhttps://github.com/kjs3829/jenkinsPractice'
                    branch: 'master'
                    credentialsId: 'Token for jenkins Practice'
            }

            post {
                success {
                    echo 'Successfully Cloned Repository'
                }

                always {
                    echo "i tried..."
                }

                cleanup {
                    echo "after all other post condition"
                }
            }


        }

        stage('Build Backend') {
            agent any
            steps {
                echo 'Build Backend'
                sh """
                ./gradlew build
                """
            }

            post {
                failure {
                    error 'This pipeline stops here...'
                }
            }
        }

        stage('Deploy Backend') {
            agent any

            steps {
                echo 'Build Backend'

                sh '''
                java -jar /home/ec2-user/jenkinsPractice/build/libs/jenkins-0.0.1-SNAPSHOT.jar
                '''
            }
        }
    }
}