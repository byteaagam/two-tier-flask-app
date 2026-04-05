pipeline {
    agent { label "dev" };

    stages {
        stage("Code Clone") {
            steps {
                git url: "https://github.com/byteaagam/two-tier-flask-app.git", branch: "master"
            }
        }

        stage("Build") {
            steps {
                sh "docker build -t two-tier-flask-app:latest ."
            }
        }

        stage("Test") {
            steps {
                echo "Developer / Tester tests likh kar dega..."
            }
        }

        stage("Push to Docker Hub") {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "dockerHubCreds",
                    usernameVariable: "dockerHubUser",
                    passwordVariable: "dockerHubPass"
                )]) {
                    sh "echo ${dockerHubPass} | docker login -u ${dockerHubUser} --password-stdin"
                    sh "docker tag two-tier-flask-app:latest ${dockerHubUser}/two-tier-flask-app:latest"
                    sh "docker push ${dockerHubUser}/two-tier-flask-app:latest"
                }
            }
        }

        stage("Deploy") {
            steps {
                sh "docker compose down || true"
                sh "docker compose up -d --build --force-recreate flask-app"
            }
        }
    }
}
