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
                powershell '& "c:\\Program Files (x86)\\VMware\\VMware Workstation\\vmrun.exe" -T ws start "c:\\Users\\jenkins\\.docker\\machine\\machines\\docker-jenkins\\docker-jenkins.vmx" ; Start-Sleep -s 5 ; docker-machine env docker-jenkins | Invoke-Expression ; docker-machine ls ; docker ps -a ; cd \"${WORKSPACE}\"\\ain-schedule-dockerdeploy_master ; docker build -t alex059/train-schedule .'
                }
            }
        }
    }
