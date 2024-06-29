pipeline{
    agent{
        label 'master'
    }

    tools {
        maven 'maven_3.9.0'
    }

    stages{
        stage('SCM Checkout'){
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/DevOps-SVC04/java-maven-war-app.git']])
            }

        }

        stage('Maven-Build'){
            steps{
                sh 'mvn clean install'
            }
        }

        // stage('Sonar Scan'){
        //     steps{
        //         withSonarQubeEnv("SonarQube") {
        //             sh "${tool("Sonar_4.8")}/bin/sonar-scanner \
        //             -Dsonar.host.url=http://ec2-3-25-175-4.ap-southeast-2.compute.amazonaws.com:9000/ \
        //             -Dsonar.login=sqp_97a364fc874b05b597e12ca633ec3aa020fba511 \
        //             -Dsonar.java.binaries=target/ \
        //             -Dsonar.projectKey=java-maven-war-app"
        //         }
        //     }
        // }

        stage('Nexus Upload'){
            steps{
                sh 'mvn -s settings.xml clean deploy'
            }
        }

        stage('deployment'){
            steps{
                sh 'ansible-playbook -i inventory deployment_playbook.yml -e "build_number=${BUILD_NUMBER}"'
            }
        }

    }
}
