//groovy script to check if there are any changes
// CODE_CHANGES = GetGitChanges()
pipeline {
    // agent runs on any avail agent
    agent any
    parameters {
        //define which selection to deploy
        choice(name: 'VERSION', choices: ['1.1.0', '1.2.0', '1.3.0'], description: '')
        booleanParam(name: 'executeTests', defaultValue: true, description: '')

    }
    tools {
        maven 'Maven'
    }
    //define your own env, available in all pipeline stages
    environment {
        NEW_VERSION = '1.3.0'
        //need to install credentials plugin
        // SERVER_CREDENTIALS = credentials('server-credentials')

    }
    stages {
        stage("build"){
            // when {
            //     expression {
            //         BRANCH_NAME == 'dev' || CODE_CHANGES == true
            //     }
            // }
            steps {
                echo 'building the application...'
                // Variable in string to be in double quote
                echo "building version ${NEW_VERSION}"
            }
        }
        
        stage("test"){
            // conditional when
            when {
                // expression {
                //     //env variable env.BRANCH_NAME
                //     //only exe when it is dev branch
                //     BRANCH_NAME == 'dev' || BRANCH_NAME == 'master'
                // }
                expression {
                    params.executeTests
                }
            }


            
            steps {
                echo "testing the application..."
            }
        }

        stage("deploy"){
            steps {
                echo "deploy the application..."
                // deploy new build to dev servers
                //we need to provide credential
                //we will need to define credential in jenkins GUI
                // echo "deploying with ${SERVER_CREDENTIALS}"
                echo "deploying version ${params.VERSION}"
            }
        }

    }

    //post execution logics to do something
    // post {
    //     always {
    //         // always run
    //     }
    //     success {

    //     }
    //     failure {

    //     }
    // }
}
