// CODE_CHANGES = getGitChanges a written ruby script to check if chenges made in the code
//  to check enviromental variables check localhost:8080/env-vars.html/ 
// to define own env variables , put in enviroment{}

// normal variables
def gv
CODE_CHANGES = true

pipeline {

    agent any
    tools {
        maven 'Maven'
        // other supported tools are :
        // gradle
        // jdk
        // for other tools u need to install the plugin
    }
    enviroment {
        NEW_VERSION = '1.3.0'
        // SERVER_CREDS = credentials('mentioned-creds')
        // u need to install the credentials plugin and create 'mentioned-creds'
        // or as best practice, mentions creds inside where u need it using wrappers With{}
    }
    parameters {
        string(name: 'VERSION', defaultValue:'', description: 'version to deploy on prod')
        choice(name: 'VERSION',choices:['1.1.0','1.1.1','1.1.2'], description: 'version to deploy on prod')
        booleanParam(name:'execute_test_stage', defaultValue: true, description:'defines wether to ex test or not')
    }

    stages {

        stage ("init"){ 
            gv = load "script.groovy"
        }

        stage("build") {
            when {
                expression {
                    params.execute_test_stage ==true
                }
            }
            steps {
                echo'building...'
                echo "building version ${NEW_VERSION}"
                sh 'mvn install'
            }
        }

        stage("test") {
            when {
                expression {
                    env.BRANCH_NAME =='dev' || CODE_CHANGES ==true
                }
            }
            steps {
                echo'testing...'
            }
        }

        stage("deploy") {

            steps {
                echo"Deploying... using the creds ${SERVER_CREDS}"
                withCredentials([
                    usernamePassword(credentials:'server-credentials',usernnameVariable: USER, passwordVariable: PWD)
                ]){
                    // use credentials USER and PASS here
                    sh "some script ${USER} ${PASS}"
                }
            }
        }
    
        stage ("Example Scripts"){ 
                script {
                    // groovy code works here, but we are just calling here
                    // gv is defined, then initiated in first stage
                    gv.someAction()
                }
        }

    }


    post {

        always {
        //
        }
        success {
        //
        }
        failure {
        //
        }
    }	
}