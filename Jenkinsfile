pipeline {
    agent {label 'application'}
    environment {
        RDS_HOSTNAME      = credentials('RDS_HOSTNAME')
        RDS_PORT          = credentials('RDS_PORT')
        RDS_USERNAME      = credentials('RDS_USERNAME')
        RDS_PASSWORD      = credentials('RDS_PASSWORD')
        REDIS_HOSTNAME    = credentials('REDIS_HOSTNAME')
        REDIS_PORT        = credentials('REDIS_PORT')
    }
    stages {
        stage('preparation') {
            steps {
               git branch: 'rds_redis', url: 'https://github.com/minasaeedbasta/jenkins_nodejs_example.git'
            }
        }
        stage('build docker image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'DOCKERHUB', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                  sh """
                    docker rm -f jenkins-nodejs-lab2
                    docker build -f dockerfile -t ${USERNAME}/jenkins-nodejs-lab2 .
                    docker run -d -p 3000:3000 \
                    -e RDS_HOSTNAME="${RDS_HOSTNAME}" \
                    -e RDS_PORT="${RDS_PORT}" \
                    -e RDS_USERNAME="${RDS_USERNAME}" \
                    -e RDS_PASSWORD="${RDS_PASSWORD}" \
                    -e REDIS_HOSTNAME="${REDIS_HOSTNAME}" \
                    -e REDIS_PORT="${REDIS_PORT}" \
                    --name jenkins-nodejs-lab2 ${USERNAME}/jenkins-nodejs-lab2
                  """
                }
            }
        }
    }
}
