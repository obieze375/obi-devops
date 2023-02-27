## Creating a Jenkinsfile


As discussed in the Getting Started section, a Jenkinsfile is a text file that contains the definition of a Jenkins Pipeline and is checked into source control. Consider the following Pipeline which implements a basic three-stage continuous delivery pipeline.

~~~~

Jenkinsfile (Declarative Pipeline)

pipeline {

    agent any

  

    stages {

        stage('Build') {

            steps {

                echo 'Building..'

            }

        }

        stage('Test') {

            steps {

                echo 'Testing..'

            }

        }

        stage('Deploy') {

            steps {

                echo 'Deploying....'

            }

        }

    }

}

~~~~

  

## Toggle Scripted Pipeline (Advanced)

  

Not all Pipelines will have these same three stages, but it is a good starting point to define them for most projects. The sections below will demonstrate the creation and execution of a simple Pipeline in a test installation of Jenkins.

  

It is assumed that there is already a source control repository set up for the project and a Pipeline has been defined in Jenkins following these instructions.

  

Using a text editor, ideally one which supports Groovy syntax highlighting, create a new Jenkinsfile in the root directory of the project.

  

The Declarative Pipeline example above contains the minimum necessary structure to implement a continuous delivery pipeline. The agent directive, which is required, instructs Jenkins to allocate an executor and workspace for the Pipeline. Without an agent directive, not only is the Declarative Pipeline not valid, it would not be capable of doing any work! By default the agent directive ensures that the source repository is checked out and made available for steps in the subsequent stages`

  

The stages directive, and steps directives are also required for a valid Declarative Pipeline as they instruct Jenkins what to execute and in which stage it should be executed.

  

For more advanced usage with Scripted Pipeline, the example above node is a crucial first step as it allocates an executor and workspace for the Pipeline. In essence, without node, a Pipeline cannot do any work! From within node, the first order of business will be to checkout the source code for this project. Since the Jenkinsfile is being pulled directly from source control, Pipeline provides a quick and easy way to access the right revision of the source code

  

## Jenkinsfile (Scripted Pipeline)

  

~~~~

node {

    checkout scm

    /* .. snip .. */

}

~~~~

  

The checkout step will checkout code from source control; scm is a special variable which instructs the checkout step to clone the specific revision which triggered this Pipeline run.

Build

For many projects the beginning of "work" in the Pipeline would be the "build" stage. Typically this stage of the Pipeline will be where source code is assembled, compiled, or packaged. The Jenkinsfile is not a replacement for an existing build tool such as GNU/Make, Maven, Gradle, etc, but rather can be viewed as a glue layer to bind the multiple phases of a project’s development lifecycle (build, test, deploy, etc) together.

  

Jenkins has a number of plugins for invoking practically any build tool in general use, but this example will simply invoke make from a shell step (sh). The sh step assumes the system is Unix/Linux-based, for Windows-based systems the bat could be used instead.

  

## Jenkinsfile (Declarative Pipeline)

  

~~~~

pipeline {

    agent any

  

    stages {

        stage('Build') {

            steps {

                sh 'make'

                archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true

            }

        }

    }

}

~~~~

  

## Toggle Scripted Pipeline (Advanced)

  

The sh step invokes the make command and will only continue if a zero exit code is returned by the command. Any non-zero exit code will fail the Pipeline.

archiveArtifacts captures the files built matching the include pattern (**/target/*.jar) and saves them to the Jenkins master for later retrieval.

Archiving artifacts is not a substitute for using external artifact repositories such as Artifactory or Nexus and should be considered only for basic reporting and file archival.

  

## Test

  

Running automated tests is a crucial component of any successful continuous delivery process. As such, Jenkins has a number of test recording, reporting, and visualization facilities provided by a number of plugins. At a fundamental level, when there are test failures, it is useful to have Jenkins record the failures for reporting and visualization in the web UI. The example below uses the junit step, provided by the JUnit plugin.

  

In the example below, if tests fail, the Pipeline is marked "unstable", as denoted by a yellow ball in the web UI. Based on the recorded test reports, Jenkins can also provide historical trend analysis and visualization.

  

## Jenkinsfile (Declarative Pipeline)

  

~~~~

pipeline {

    agent any

  

    stages {

        stage('Test') {

            steps {

                /* `make check` returns non-zero on test failures,

                * using `true` to allow the Pipeline to continue nonetheless

                */

                sh 'make check || true'

                junit '**/target/*.xml'

            }

        }

    }

}

~~~~

  

## Toggle Scripted Pipeline (Advanced)

  

Using an inline shell conditional (sh 'make || true') ensures that the sh step always sees a zero exit code, giving the junit step the opportunity to capture and process the test reports. Alternative approaches to this are covered in more detail in the Handling Failures section below.

junit captures and associates the JUnit XML files matching the inclusion pattern (**/target/*.xml).

Deploy

Deployment can imply a variety of steps, depending on the project or organization requirements, and may be anything from publishing built artifacts to an Artifactory server, to pushing code to a production system.

  

At this stage of the example Pipeline, both the "Build" and "Test" stages have successfully executed. In essense, the "Deploy" stage will only execute assuming previous stages completed successfully, otherwise the Pipeline would have exited early.

  

## Jenkinsfile (Declarative Pipeline)

~~~~

pipeline {

    agent any

  

    stages {

        stage('Deploy') {

            when {

              expression {

                currentBuild.result == null || currentBuild.result == 'SUCCESS'

              }

            }

            steps {

                sh 'make publish'

            }

        }

    }

}

~~~~

  

## Toggle Scripted Pipeline (Advanced)

  

Accessing the currentBuild.result variable allows the Pipeline to determine if there were any test failures. In which case, the value would be UNSTABLE.

Assuming everything has executed successfully in the example Jenkins Pipeline, each successful Pipeline run will have associated build artifacts archived, test results reported upon and the full console output all in Jenkins.

  

A Scripted Pipeline can include conditional tests (shown above), loops, try/catch/finally blocks and even functions. The next section will cover this advanced Scripted Pipeline syntax in more detail.

  

## Advanced Syntax for Pipeline

  

## String Interpolation

  

Jenkins Pipeline uses rules identical to Groovy for string interpolation. Groovy’s String interpolation support can be confusing to many newcomers to the language. While Groovy supports declaring a string with either single quotes, or double quotes, for example:

  

~~~~

def singlyQuoted = 'Hello'

def doublyQuoted = "World"

Only the latter string will support the dollar-sign ($) based string interpolation, for example:

  

def username = 'Jenkins'

echo 'Hello Mr. ${username}'

echo "I said, Hello Mr. ${username}"

Would result in:

  

Hello Mr. ${username}

~~~~

  

I said, Hello Mr. Jenkins

Understanding how to use string interpolation is vital for using some of Pipeline’s more advanced features.

  

## Working with the Environment

  

Jenkins Pipeline exposes environment variables via the global variable env, which is available from anywhere within a Jenkinsfile. The full list of environment variables accessible from within Jenkins Pipeline is documented at localhost:8080/pipeline-syntax/globals#env, assuming a Jenkins master is running on localhost:8080, and includes:

  

BUILD_ID

The current build ID, identical to BUILD_NUMBER for builds created in Jenkins versions 1.597+

  

JOB_NAME

Name of the project of this build, such as "foo" or "foo/bar".

  

JENKINS_URL

Full URL of Jenkins, such as example.com:port/jenkins/ (NOTE: only available if Jenkins URL set in "System Configuration")

  

Referencing or using these environment variables can be accomplished like accessing any key in a Groovy Map, for example:

  

## Jenkinsfile (Declarative Pipeline)

  

~~~~

pipeline {

    agent any

    stages {

        stage('Example') {

            steps {

                echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"

            }

        }

    }

}

  

~~~~

  
  

## Toggle Scripted Pipeline (Advanced)

  

## Setting environment variables

  

Setting an environment variable within a Jenkins Pipeline is accomplished differently depending on whether Declarative or Scripted Pipeline is used.

  

Declarative Pipeline supports an environment directive, whereas users of Scripted Pipeline must use the withEnv step.

  

~~~~

Jenkinsfile (Declarative Pipeline)

pipeline {

    agent any

    environment {

        CC = 'clang'

    }

    stages {

        stage('Example') {

            environment {

                DEBUG_FLAGS = '-g'

            }

            steps {

                sh 'printenv'

            }

        }

    }

}

~~~~

  

## Toggle Scripted Pipeline (Advanced)

  

An environment directive used in the top-level pipeline block will apply to all steps within the Pipeline.

  

An environment directive defined within a stage will only apply the given environment variables to steps within the stage.

  

## Parameters

  

Declarative Pipeline supports parameters out-of-the-box, allowing the Pipeline to accept user-specified parameters at runtime via the parameters directive. Configuring parameters with Scripted Pipeline is done with the properties step, which can be found in the Snippet Generator.

  

If you configured your pipeline to accept parameters using the Build with Parameters option, those parameters are accessible as members of the params variable.

  

Assuming that a String parameter named "Greeting" has been configuring in the Jenkinsfile, it can access that parameter via ${params.Greeting}:

  

~~~~

Jenkinsfile (Declarative Pipeline)

pipeline {

    agent any

    parameters {

        string(name: 'Greeting', defaultValue: 'Hello', description: 'How should I greet the world?')

    }

    stages {

        stage('Example') {

            steps {

                echo "${params.Greeting} World!"

            }

        }

    }

}

~~~~

  

## Toggle Scripted Pipeline (Advanced)

  

## Handling Failures

  

Declarative Pipeline supports robust failure handling by default via its post section which allows declaring a number of different "post conditions" such as: always, unstable, success, failure, and changed. The Pipeline Syntax section provides more detail on how to use the various post conditions.

  

~~~~

Jenkinsfile (Declarative Pipeline)

pipeline {

    agent any

    stages {

        stage('Test') {

            steps {

                sh 'make check'

            }

        }

    }

    post {

        always {

            junit '**/target/*.xml'

        }

        failure {

            mail to: team@example.com, subject: 'The Pipeline failed :('

        }

    }

}

~~~~

  

## Toggle Scripted Pipeline (Advanced)

  

Scripted Pipeline however relies on Groovy’s built-in try/catch/finally semantics for handling failures during execution of the Pipeline.

  

In the Test example above, the sh step was modified to never return a non-zero exit code (sh 'make check || true'). This approach, while valid, means the following stages need to check currentBuild.result to know if there has been a test failure or not.

  

An alternative way of handling this, which preserves the early-exit behavior of failures in Pipeline, while still giving junit the chance to capture test reports, is to use a series of try/finally blocks:

  

## Using multiple agents

  

In all the previous examples, only a single agent has been used. This means Jenkins will allocate an executor wherever one is available, regardless of how it is labeled or configured. Not only can this behavior be overridden, but Pipeline allows utilizing multiple agents in the Jenkins environment from within the same Jenkinsfile, which can helpful for more advanced use-cases such as executing builds/tests across multiple platforms.

  

In the example below, the "Build" stage will be performed on one agent and the built results will be reused on two subsequent agents, labelled "linux" and "windows" respectively, during the "Test" stage.

  

~~~~

Jenkinsfile (Declarative Pipeline)

pipeline {

    agent none

    stages {

        stage('Build') {

            agent any

            steps {

                checkout scm

                sh 'make'

                stash includes: '**/target/*.jar', name: 'app'

            }

        }

        stage('Test on Linux') {

            agent {

                label 'linux'

            }

            steps {

                unstash 'app'

                sh 'make check'

            }

            post {

                always {

                    junit '**/target/*.xml'

                }

            }

        }

        stage('Test on Windows') {

            agent {

                label 'windows'

            }

            steps {

                unstash 'app'

                bat 'make check'

            }

            post {

                always {

                    junit '**/target/*.xml'

                }

            }

        }

    }

}

~~~~

  

## Toggle Scripted Pipeline (Advanced)

  

The stash step allows capturing files matching an inclusion pattern (**/target/*.jar) for reuse within the same Pipeline. Once the Pipeline has completed its execution, stashed files are deleted from the Jenkins master.

The parameter in agent/node allows for any valid Jenkins label expression. Consult the Pipeline Syntax section for more details.

unstash will retrieve the named "stash" from the Jenkins master into the Pipeline’s current workspace.

The bat script allows for executing batch scripts on Windows-based platforms.

  

## Optional step arguments

  

Pipeline follows the Groovy language convention of allowing parentheses to be omitted around method arguments.

  

Many Pipeline steps also use the named-parameter syntax as a shorthand for creating a Map in Groovy, which uses the syntax [key1: value1, key2: value2]. Making statements like the following functionally equivalent:

  

~~~~

git url: 'git://example.com/amazing-project.git', branch: 'master'

git([url: 'git://example.com/amazing-project.git', branch: 'master'])

~~~~

  

For convenience, when calling steps taking only one parameter (or only one mandatory parameter), the parameter name may be omitted, for example:

~~~~

sh 'echo hello' /* short form  */

sh([script: 'echo hello'])  /* long form */

~~~~

  

## Advanced Scripted Pipeline

  

Scripted Pipeline is a domain-specific language [3] based on Groovy, most Groovy syntax can be used in Scripted Pipeline without modification.

  

## Executing in parallel

  

The example in the section above runs tests across two different platforms in a linear series. In practice, if the make check execution takes 30 minutes to complete, the "Test" stage would now take 60 minutes to complete!

  

Fortunately, Pipeline has built-in functionality for executing portions of Scripted Pipeline in parallel, implemented in the aptly named parallel step.

  

Refactoring the example above to use the parallel step:

  

~~~~

Jenkinsfile (Scripted Pipeline)

stage('Build') {

    /* .. snip .. */

}

  

stage('Test') {

    parallel linux: {

        node('linux') {

            checkout scm

            try {

                unstash 'app'

                sh 'make check'

            }

            finally {

                junit '**/target/*.xml'

            }

        }

    },

    windows: {

        node('windows') {

            /* .. snip .. */

        }

    }

}

~~~~

  

Instead of executing the tests on the "linux" and "windows" labelled nodes in series, they will now execute in parallel assuming the requisite capacity exists in the Jenkins environment.

  

https://rtyler.github.io/jenkins.io/doc/book/pipeline/jenkinsfile/

  

https://www.guru99.com/jenkins-pipeline-tutorial.html