```groovy
pipeline {
    agent any

    stages {
        stage('Check Environment') {
            steps {
                bat 'git --version'
                bat '"C:\\Users\\pc\\AppData\\Local\\Programs\\Python\\Python314\\python.exe" --version'
                bat '"C:\\Users\\pc\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe" --version'
            }
        }

        stage('Download GitHub Project') {
            steps {
                deleteDir()

                bat 'curl.exe -fL --retry 2 --connect-timeout 15 --max-time 120 -o project.zip https://codeload.github.com/ammu780/Dynamic-Class-Track/zip/refs/heads/Main'

                powershell '''
                    Expand-Archive -Path project.zip -DestinationPath extracted -Force
                    $folder = Get-ChildItem extracted -Directory | Select-Object -First 1
                    Copy-Item "$($folder.FullName)\\*" . -Recurse -Force
                    Remove-Item project.zip, extracted -Recurse -Force
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                bat '"C:\\Users\\pc\\AppData\\Local\\Programs\\Python\\Python314\\python.exe" -m pip --version'
                bat '"C:\\Users\\pc\\AppData\\Local\\Programs\\Python\\Python314\\python.exe" -m pip install -r requirements.txt'
            }
        }

        stage('Run Automated Tests') {
            steps {
                bat '"C:\\Users\\pc\\AppData\\Local\\Programs\\Python\\Python314\\python.exe" -m pytest tests -v'
            }
        }

        stage('Inspect Project') {
            steps {
                bat 'dir'
                bat 'if exist requirements.txt (echo requirements.txt found) else (echo requirements.txt not found)'
                bat 'if exist Dockerfile (echo Dockerfile found) else (echo Dockerfile not found)'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat '"C:\\Users\\pc\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe" build -t dynamic-class-track:latest .'
            }
        }

        stage('Verify Docker Image') {
            steps {
                bat '"C:\\Users\\pc\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe" image inspect dynamic-class-track:latest'
            }
        }
    }
}
```