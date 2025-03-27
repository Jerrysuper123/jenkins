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




