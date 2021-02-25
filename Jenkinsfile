pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                bat './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('PowerShell command for docker-machine') {
            steps {
                powershell 'vmrun -T ws start "c:\\Users\\sahka\\.docker\\machine\\machines\\docker\\docker.vmx"'
                powershell 'Start-Sleep -s 60'
                powershell 'docker-machine env docker | Invoke-Expression'
            }
        }
        stage('Build Docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    app = docker.build("alex059/train-schedule")
                    app.inside {
                        sh 'echo $(curl localhost:8080)'
                    }
                }
            }
        }
    }
}
