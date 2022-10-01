pipeline {
    agent any
    environment {
        PATH="/opt/liquibase:${env.PATH}"
    }
    tools {
        maven "Maven3.8.6"
    }

    stages {
       /* 
        stage('Dependency Check') {
            steps {
                dir('.'){
                    dependencyCheck additionalArguments: ''' 
                        -o "./" 
                        -s "./"
                        -f "ALL" 
                        --prettyPrint''', odcInstallation: 'DepCheck_7.2.1'
                    dependencyCheckPublisher pattern: 'dependency-check-report.xml'
                }
               
            }
        }

        stage('Analyze'){
            steps{
                withSonarQubeEnv('MiSonarQube'){
                    sh "mvn clean package sonar:sonar \
                            -Dsonar.projectKey=21_MyCompany_Microservice \
                            -Dsonar.projectName=21_MyCompany_Microservice \
                            -Dsonar.sources=src/main \
                            -Dsonar.coverage.exclusions=/*TO.java,/DO.java,/example/web//,/example/persistence//,/example/commons//,/example/model//* \
                            -Dsonar.coverage.jacoco.xmlReportPaths=target/site/jacoco/jacoco.xml \
                            -Djacoco.output=tcpclient \
                            -Djacoco.address=127.0.0.1 \
                            -Djacoco.port=10001"
                }
            }
        }
        

        stage('Build'){
            steps{
                sh "docker build -t microservicio ."
            }
        }*/

        stage('Push image') {
            steps {
                withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'docker_nexus', usernameVariable: 'usrnexus', passwordVariable: 'pswdnexus']]) {
                  sh 'docker login -u $usrnexus -p $pswdnexus 192.168.5.125:8083'
                  sh "docker tag microservicio:latest 192.168.5.125:8083/repository/docker-private/microservicio:latest"
                  sh "docker push 192.168.5.125:8083/repository/docker-private/microservicio:latest"
                }
            }
        }

        stage('Liquibase') {
            steps {
                dir("liquibase/"){
                    sh 'liquibase --changeLogFile="changesets/db.changelog-master.xml" update'
                }
            }
        }
        

    }
}
