@Library('shared') _ 
pipeline{
    
    agent any

    environment {
        SONAR_HOST_URL = 'http://localhost:9000'
        SONAR_TOKEN = credentials('SonarQube-Server')
        PROJECT_KEY = 'my-app-project'  // Your actual SonarQube project key
    }


    
    stages{
        stage('Clean') {  // Clean before building
    steps {
        cleanWs()
        echo 'Workspace cleaned before build!'
    }
}
        stage("Code Clone"){
            steps{
               script{
                   clone("https://github.com/LondheShubham153/two-tier-flask-app.git", "master")
               }
            }
        }
        stage("Trivy File System Scan"){
            steps{
                script{
                    trivy_fs()
                }
            }
        }
        stage("Code Build"){
            steps{
                sh "docker build -t two-tier-flask-app ."
            }
            
        }
        stage('Run SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar-qube') {  // Uses your configured SonarQube server in Jenkins
                    sh """
                    docker run --rm \
                    -e SONAR_HOST_URL=$SONAR_HOST_URL \
                    -e SONAR_LOGIN=$SONAR_TOKEN \
                    -v \$(pwd):/usr/src \
                    sonarsource/sonar-scanner-cli \
                    -Dsonar.projectKey=$PROJECT_KEY \
                    -Dsonar.sources=.
                    """
                }
            }
        }
        
        stage("Test"){
            steps{
                echo "Developer / Tester tests likh ke dega..."
            }
            
        }
        stage("Push to Docker Hub"){
            steps{
                script{
                    docker_push("dockerhubcredss","two-tier-flask-app")
                }  
            }
        }
        stage("Deploy"){
            steps{
                sh "docker compose up -d --build flask-app"
            }
        }
    }

post{
        success{
            script{
                emailext from: 'mentor@trainwithshubham.com',
                to: 'mentor@trainwithshubham.com',
                body: 'Build success for Demo CICD App',
                subject: 'Build success for Demo CICD App'
            }
        }
        failure{
            script{
                emailext from: 'mentor@trainwithshubham.com',
                to: 'mentor@trainwithshubham.com',
                body: 'Build Failed for Demo CICD App',
                subject: 'Build Failed for Demo CICD App'
            }
        }
    }
}
