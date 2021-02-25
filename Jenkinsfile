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
        stage('PowerShell command for docker-machine') {
            steps {
                powershell '& "c:\\Program Files (x86)\\VMware\\VMware Workstation\\vmrun.exe" -T ws start "c:\\Users\\jenkins\\.docker\\machine\\machines\\docker-jenkins\\docker-jenkins.vmx"'
                powershell 'Start-Sleep -s 5'
                powershell 'docker-machine env docker-jenkins | Invoke-Expression'
            }
        }
        stage('Build Docker Image') {
            when {
                branch 'none'
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
