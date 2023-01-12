node{
    
    def Maven_Home = tool name: "maven3.8.1"
    
    stage('CodeCheckout'){
        git branch: 'development ', credentialsId: '279dccca-f6f4-4de9-96c3-122ffad0c2e1', url: 'https://github.com/A-Chandrasekhar/maven-web-application.git'
    }
    stage('Build'){
    sh "${Maven_Home}/bin/mvn clean package"    
    }
    
    stage('Generate SonarQube Report'){
    sh "${Maven_Home}/bin/mvn clean sonar:sonar"    
    }
    
    stage('Upload ArtfactIntoNexus'){
    sh "${Maven_Home}/bin/mvn clean deploy"    
    }
    
    stage('DeployAppIntoTomcatServer'){
        sshagent(['51d1f914-f3ba-44da-bbbc-50caeb73bb72']) {
            sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@65.0.91.168:/opt/tomcat9/webapps/"
        }
    }
    
    stage('Email Notifiaction'){
        emailext body: '''Build Over
        Regards
        Chandrasekhar''', subject: 'Build Over!', to: 'al.chandrasekhar04@gmail.com'
    }
}
