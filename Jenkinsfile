node {
    notify('Started')
    try {
        stage('checkout') {
            checkout scm
            //git 'https://github.com/sjyeye/jenkins2-course-spring-boot-1.git'
        }
        
        def project_path = "spring-boot-samples/spring-boot-sample-atmosphere"
        dir(project_path) {
            stage('compiling, test, packageing') {
                sh label: '', script: 'sed -i \'s+http://repo+https://repo+g\' ../pom.xml'
                sh label: '', script: 'mvn clean package'
            }
            stage('archiving') {
                archiveArtifacts "target/*.jar"
            }
        }
        notify('Success')
    } catch (err) {
        notify("Caught: ${err}")
        currentBuild.result = 'FAILURE'
    }
}

def notify(status){
    emailext body: """<p>${status}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
        <p>Check console output at <a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a></p>""",
    subject: "${status}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
    to: 'shijhuan@cisco.com'
}
