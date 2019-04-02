#!groovy

pipeline {
    agent none
    stages {
        stage('Hello....') {
            steps {
                script {
                    openshift.withCluster( 'okd-master1' ) {
                        // Prints a list of current service accounts to the console
                        echo "Current project is ${openshift.project()}"
                    }
                }
            }
        }
        stage('Build App....') {
            steps {
                script {
                   openshift.withCluster('okd-master1') {
                        openshift.withProject() {
                            openshift.newApp("--name=hackazon", "--docker-image=cjunwchen/hackazon")
                        }
                    }
                }
            }
        }
        stage('Deploy....') {
            steps {
                 echo "Deploying hackazon ...."
            }
        }
    }
}
