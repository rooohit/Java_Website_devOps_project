def COLOR_MAP = [
    'SUCCESS': 'good',
    'FAILURE': 'danger',
]
pipeline {
    agent any 
    
    environment {
        NEXUS_PASS = 'test1234'
    }
    stages {

        stage ("Setup Parameters") {
            steps {
                script {
                    properties ([
                        parameters ([
                            string (
                                defaultValue: '',
                                name: 'BUILD',
                            ),
                            string (
                                defaultValue: '',
                                name: 'TIME',
                            )
                        ])
                    ])
                }
            }

        }
        stage ("Ansible Deploy to Prod") {
            steps {
                ansiblePlaybook ([
                    inventory: 'ansible/prod.inventory',
                    playbook: 'ansible/site.yml',
                    installation: 'ansible',
                    colorized: true,
                    credentialsId: 'applogin',
                    disableHostKeyChecking: true,
                    extraVars : [
                        USER: "admin",
                        PASS: "${NEXUS_PASS}",
                        nexusip: '172.31.34.61',
                        reponame: 'vprofile-release',
                        groupid: 'QA',
                        time: "${env.TIME}",
                        build: "${env.BUILD}",
                        artifactid: 'vproapp',
                        vprofile_version: "vproapp-${env.BUILD}-${env.TIME}.war"
                    ]
                ])
            }
        }

    }
    post {
        always {
            echo 'Slack Notification.'
            slackSend channel: '#jenkinscicd',
            color: COLOR_MAP[currentBuild.currentResult],
            message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER} \n More info at: ${env.BUILD_URL}"
        }
    }

}