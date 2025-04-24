// This is a simple Jenkins pipeline script that defines three stages: Build, Test, and Deploy.
// Each stage contains a step that prints a message to the console.
// The pipeline is defined using the declarative syntax, which is a more structured way to define Jenkins pipelines.
// The 'agent any' directive specifies that the pipeline can run on any available agent.
// The 'stages' block contains the individual stages of the pipeline, each defined with a 'stage' block.
// The 'steps' block within each stage contains the actual commands to be executed.
// This pipeline can be used as a starting point for building, testing, and deploying applications in Jenkins.
// You can customize the steps in each stage to fit your specific build, test, and deployment processes.
// Additionally, you can define environment variables, parameters, and post-build actions to enhance the functionality of your pipeline.The pipeline can be triggered manually or automatically based on events such as code commits or pull requests.
// You can also integrate it with other tools and services, such as version control systems, testing frameworks, and deployment platforms.
// This allows for a seamless and automated workflow, reducing the need for manual intervention and increasing efficiency.
// The pipeline can be further enhanced by adding error handling, notifications, and reporting features.
// Overall, this Jenkins pipeline script provides a solid foundation for automating the build, test, and deployment processes in a continuous integration and continuous deployment (CI/CD) environment.
// It can be easily extended and customized to meet the specific needs of your project or organization.
// You can also use Jenkins plugins to add additional functionality, such as integration with cloud services, containerization, and monitoring.
// By using this pipeline, you can ensure that your code is consistently built, tested, and deployed, leading to higher quality software and faster release cycles.


pipeline{

    // agent {docker{image 'maven:3.6.3'}}
    agent any

    environment{
        dockerHome = tool 'Dockerjee'
        mavenHome = tool 'Mavenjee'
        PATH = "$dockerHome/bin:$mavenHome/bin:PATH"
    }

    stages {
        stage('Checkout') {
            steps {
				
                echo 'Building..'

                echo "BUILD PATH - $PATH"
                echo "BUILD_NUMBER - $env.BUILD_NUMBER "
                echo "build_id : $env.build_id"
                echo "JOB_NAME : $env.JOB_NAME"
                echo "BUILD_TAG : $env.BUILD_TAG"
                echo "BUILD_URL : $env.BUILD_URL"
            }
        }

        stage('Compile'){
            steps{
                sh 'mvn clean compile'
            }
        }

      /*  stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Integration Test') {
            steps {
                sh 'mvn failsafe:integration-test'
            }
        }*/

        stage('Build docker image') {
    steps {
        script {
            dockerimage = docker.build("jeevith2/mmv3-currency-exchange-service:${BUILD_TAG}")
        }
    }
}

         stage('Push docker image') {
    steps {
        script {
            docker.withRegistry('', 'dockerhub') {
                dockerimage.push()
                dockerimage.push('latest')
            }
        }
    }
}
    }


	
//post helps us to run some steps after the pipeline execution
//always, success, and failure are conditions that determine when the steps inside them will be executed
//always will run regardless of the pipeline's success or failure
//success will run only if the pipeline is successful
//failure will run only if the pipeline fails
//You can add more conditions like unstable, aborted, etc.
//You can also use post to send notifications, clean up resources, or perform any other necessary actions after the pipeline execution

post{
	always{
		echo 'This line will always run and it will get print'
	}

	success{
		echo 'This line will run only if the pipeline is successful'
	}

	failure{
		echo 'This line will run only if the pipeline fails'
	}
}
}
