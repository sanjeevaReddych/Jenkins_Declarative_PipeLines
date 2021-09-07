# Jenkins_Declarative_PipeLines
Best Practices for Jenkins PipeLine

## Create Environment Variable
Create a Jenkins parameter **Build_by** which later can be accessed as **env.Build_by**
```
environment {
                Build_by = "Bazel"
      }
```

## Create Parameter
Create a Jenkins parameter **Branch** which later can be accessed as **param.Branch**
```
parameters {
                string defaultValue: 'main', name: 'Branch', trim: true
                }
```

## Run Parallel Job
Below syntax runs the main_branch and bs_branch in parallelly inside the Jenkins.
```
pipeline {
    agent any
    
    stages {
        stage('paralle job') {
            parallel {
                stage('main_branch'){
                    steps{
                        build job: 'cpp_test', parameters: [string(name: 'Branch', value: 'main')]
                    }
                }
                
                stage('bs_branch'){
                    steps{
                        build job: 'cpp_test', parameters: [string(name: 'Branch', value: 'bs')]
                    }
                }
            }
        }
    
        }
}
```
## Conditional Operations
Choosing the conditional operation using If logic
```
script {
                    if (env.Build_by == 'Bazel'){
                        echo "Env setup for Bazel"
                        sh '''
                            sudo apt install npm
                            sudo npm install -g @bazel/bazelisk
                            bazel --version
                            '''
                        echo "Building project using Bazel "
                        sh '''
                            cd cpp-tutorial/stage1
                            bazel build //main:hello-world
                            '''
                    } else {
                    echo "This build system is not configured"
                    }
                }
                
```

