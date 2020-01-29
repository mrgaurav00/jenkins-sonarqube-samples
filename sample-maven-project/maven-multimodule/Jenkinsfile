node {
    notify("Started")
    
    try {
        stage("checkout") {
            //git "https://github.com/mrgaurav00/jenkins-sonarqube-samples.git"
			checkout scm
        }
        
        dir("sample-maven-project/maven-multimodule") {
            stage("build") {
                withSonarQubeEnv("Sonarqube Server") {
                    sh "mvn clean package sonar:sonar"
                }
            }
            
            stage("archival") {
                archiveArtifacts "module?/target/*.?ar"
            }
        }
        
        notify("Success")
    } catch (err) {
        notify("Error ${err}")
        currentBuild.result = "FAILURE"
    }
}

def notify(status) {
    emailext(
        to: "mrgaurav00@trash-mail.com", 
        subject: "${status}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
        body: "<p>${status}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p><p>Check console output at '${BUILD_URL}' to view the results."
    )
}
