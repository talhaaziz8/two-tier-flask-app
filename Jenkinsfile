@Library('shared') _ 
pipeline{
    
    agent any
    
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
        stage("Build"){
            steps{
                sh "docker build -t two-tier-flask-app ."
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
