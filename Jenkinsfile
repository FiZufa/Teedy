pipeline {
    agent any
    stages {
        // Package the application using Maven
        stage('Package') {
            steps {
                // Checkout code from the main branch
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/FiZufa/Teedy.git']])

                // Run Maven to package the application, skipping unit tests
                sh 'mvn -B -DskipTests clean package'
            }
        }
        // Build the Docker image
        stage('Building image') {
            steps {
                // Build the Docker image using the Dockerfile in the project
                script {
                    dockerImage = docker.build("fitria06/teedy-image")
                }
            }
        }
        // Upload the Docker image to Docker Hub
        stage('Upload image') {
            steps {
                // Push the Docker image to Docker Hub
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'labteedy2024') {
                        dockerImage.push("latest")
                    }
                }
            }
        }
        // Run Docker containers
        stage('Run containers') {
            steps {
                script {
                    // Run three containers with specified port mappings
                    def container1 = docker.run("fitria06/teedy-image", "-p 8082:80")
                    def container2 = docker.run("fitria06/teedy-image", "-p 8083:80")
                    def container3 = docker.run("fitria06/teedy-image", "-p 8084:80")
                    // Sleep for 30 seconds to allow containers to execute
                    sleep 30
                    // Stop the containers after running
                    container1.stop()
                    container2.stop()
                    container3.stop()
                }
            }
        }
    }
}
