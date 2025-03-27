# jenkins

## what is jenkins file?
instead of user interface, write the config in a file. it is pipeline as a code

We place jenkins file in the repository

source: https://www.youtube.com/watch?v=7KCS70sCoK0

## pipeline syntax
can be written in scripted or declarative syntax

scripted syntax
```
node {
  groovy script
}
```
groovy is a bit diffcult to start with

so declarative is easier to start with, but not as powerful as scripted syntax
```
pipeline {
  agent any
//different stages you want to run, build, test and deploy stages
  stages {
    stage("build") {
//with steps, are some commands to exe scripts in the jenkins
// example sh "node install"
      steps {}
    }
  }
  post {
    //always do sth, you can send email to colleague
    always {
      
    }
    success {}
    failure {}
  }
}
```
## 2 step go to jenkins and create a new build 
Then u can add a git repo
credentials (create and use them)

then it scan all branches, and only find jenkins file on dev branch, so it only build on dev branch

## go into "configuration"
implicitly checking out the code

it will auto spot jenkins file in my source code. and stay on dev branch

## you can define "post" attributes
do something after all the stages are done, always, success or failure conditions

## you can also define conditional for each stage

```
//getGitChanges is a funct you defined using groovy scripts to check if your code has changed or not
CODE_CHANGES = getGitChanges()

pipeline {
  agent any
  stages {
    stage("build") {
        when {
        expression {
    //branch_name is the env variable, only code has changed
          BRANCH_NAME == "dev' || CODE_CHANGES == true
        }
      }
      steps {
        //below will only execute when branch name is dev
          echo 'building the application...'
      }
    }
    stage("test"){
      when {
        expression {
    //branch_name is the env variable
          BRANCH_NAME == "dev' || BRANCH_NAME == "master'
        }
      }
      steps {
        //below will only execute when branch name is dev
          echo 'testing the application...'
      }
    }
  }
  post {
    always {
      
    }
    success {}
    failure {}
  }
}

## environment variables in jenkinsfile
load below the see the list of env var, you can use them in jenkins file

```
localhost:8080/env-vars.html/
```
## you can define your own environment variable

FOR credentials to work, install credential and credential binding plugin in jenkins

```
pipeline {
  agent any
  environment {
    //usu cal this by extracting from the code
    NEW_VERSION = '1.3.0'
    //take id as the credential id/reference
    //need to install a plug in to use this
    SERVER_CREDENTIAL = credential('')
  }
  stages {
    stage("build") {
      steps {
        echo 'building the application ... '
        //use double quote when concatenate the string
        echo "building version ${NEW_VERSION}"
      }
    }
    stage("deploy"){
      steps {
        echo "deploy with ${SERVER_CREDENTIAL}"
        //OR YOU CAN USE withCredentials() wrapper to get the username and pw
        withCredentials(
          [usernamepassword(credentials:'server-credentials',usernameVariable: USER, passwordVariable: PWD)]
        ){
          //with this block i can use the username and password
          sh "some script ${USER} ${PWD}"
        }
      }
    }
  }
  post {
    always {
    }
    success {}
    failure {}
  }
}
```

you can define credentials in jenkinsfile then use them in jenkinfiles
