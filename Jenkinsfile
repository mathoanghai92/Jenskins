pipeline {

    agent any

    tools { 
        maven 'my-maven' 
    }
    environment {
        MYSQL_ROOT_LOGIN = credentials('mysql-root-login')
    }
    stages {

        stage('Build with Maven') {
            steps {
                sh 'mvn --version'
                sh 'java -version'
                sh 'mvn clean package -Dmaven.test.failure.ignore=true'
            }
        }

        stage('Packaging/Pushing imagae') {

            steps {
                withDockerRegistry(credentialsId: 'dockerhub', url: 'https://index.docker.io/v1/') {
                    sh 'docker build -t mathoanghai92/springboot .'
                    sh 'docker push mathoanghai92/springboot'
                }
            }
        }

        stage('Deploy MySQL to DEV') {
            steps {
                withCredentials([string(credentialsId: 'mysql-root-login', variable: 'mysqlRootPassword')]) {
                        echo 'Deploying and cleaning'
                        sh 'docker image pull mysql:8.0'
                        sh 'docker network create dev || echo "this network exists"'
                        sh 'docker container stop db-mysql || echo "this container does not exist" '
                        sh 'echo y | docker container prune '
                        sh 'docker volume rm db-mysql-data || echo "no volume"'
                        sh "docker run --name db-mysql --rm --network dev -v mysql-data:/var/lib/mysql -e mysql-root-login=${mysqlRootPassword} -e MYSQL_DATABASE=db_example -d mysql:8.0"
                        sh 'sleep 20'
                        sh 'docker exec -i db-mysql mysql --user=root --password=${mysqlRootPassword} < script'
                }
            }
        }
        stage('Deploy Spring Boot to DEV') {
            steps {
                echo 'Deploying and cleaning'
                sh 'docker image pull mathoanghai92/springboot'
                sh 'docker container stop mathoanghai92-springboot || echo "this container does not exist" '
                sh 'docker network create dev || echo "this network exists"'
                sh 'echo y | docker container prune '

                sh 'docker container run -d --rm --name mathoanghai92-springboot -p 8081:8080 --network dev mathoanghai92/springboot'
            }
        }
 
    }
    post {
        // Clean after build
        always {
            cleanWs()
        }
    }
}
