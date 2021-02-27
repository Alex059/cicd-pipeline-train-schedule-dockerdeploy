pipeline {
    agent any
    stages {
        stage('Build') {
            when {
                branch 'none'
            }
            steps {
                echo 'Running build automation'
                bat './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('Build Docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    withEnv(['JENKINS_NODE_COOKIE=dontkillme']) {
                        powershell '& "c:\\Program Files (x86)\\VMware\\VMware Workstation\\vmrun.exe" -T ws start "c:\\Users\\jenkins\\.docker\\machine\\machines\\docker-jenkins\\docker-jenkins.vmx" ; Start-Sleep -s 5 ; docker-machine env docker-jenkins | Invoke-Expression ; docker-machine ls ; docker ps -a'
                        app = docker.build("alex059/train-schedule")
                        app.inside {
                            sh 'echo $(curl localhost:8080)'
                        }
                    }
                }
            }
        }
    }
}
