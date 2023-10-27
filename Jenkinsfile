pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                script {
                    // Maven을 사용하는 경우 'mvn package'를, Gradle을 사용하는 경우 'gradle build'를 실행해 JAR 파일을 빌드합니다.
                    sh 'gradle build'
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    // 빌드 결과물을 Ubuntu 서버에 복사합니다. 이때 scp 명령어를 사용할 수 있습니다.
                    // 'user'와 'host'는 실제 서버의 사용자 이름과 호스트를, 'source'는 빌드 결과물의 경로를, 'destination'은 서버에서의 저장 위치를 나타냅니다.

                    withCredentials([usernamePassword(credentialsId: 'ubuntu-rasp', usernameVariable: 'USER', passwordVariable: 'PASSWORD')]) {
                        sh 'sshpass -p $PASSWORD $USER@59.29.102.247 kill -9 $(lsof -t -i:8000)'
                        sh 'scp /build/lib/demo-0.0.1-SNAPSHOT.jar $USER@59.29.102.247:/home/$USER/jenkins'
                        sh 'ssh -p $PASSWORD $USER@59.29.102.247 java -jar ./jenkins/demo-0.0.1-SNAPSHOT.jar '
                    }
                }
            }
        }
    }
}
