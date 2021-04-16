pipeline {    
agent any
    stages {
        stage('Checkout Code') {
            steps {
                cleanWs()
                git branch: "master", url:'https://github.com/sushilc147/ROBOTTESTING.git'
            }
        }
        stage('Execute Tests') {
            steps{
                sh '''
                docker-compose run robot
                '''
                sh '''cat pybot_stdout.out''' 
                //sh 'docker run -v ${PWD}/:/ROBOTTESTING/robot/ interworks/rfrunner'
            }
        }
        stage('Proccess Results') {		
            steps {
                script{
                    bat 'del "Results\\*.zip'
                    zip zipFile: 'results/results.zip', archive: false, dir: 'results', glob: '*.html'
                    step(
                        [
                            $class              : 'RobotPublisher',
                            outputPath          : 'results',
                            outputFileName      : "output.xml",
                            reportFileName      : 'report.html',
                            logFileName         : 'log.html',
                            disableArchiveOutput : false,
                            passThreshold       : 95.0,
                            unstableThreshold   : 90.0,
                            otherFiles          : "**/*.png,**/*.jpg",
                        ]
                    )
                emailext body: '${SCRIPT, template="robot.template"}', subject: "[Jenkins] Robot Framework testresults for Docker Demo Project", to: 'sushilc147a@gmail.com', recipientProviders: [[$class: 'CulpritsRecipientProvider']], attachmentsPattern: 'results/results.zip'
                }
            }
        }
    }
    post {
        always {
            archive (includes: 'results/*.html')
        }
    }
}
