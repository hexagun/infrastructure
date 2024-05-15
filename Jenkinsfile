pipeline {
    agent any
    

    environment {
        // Define environment variables
        CHART_NAME = 'hexagun-webapp' // Name of the Helm chart
        DEV_NAMESPACE = 'dev' // Namespace for development environment
        STAGING_NAMESPACE = 'staging' // Namespace for staging environment
    }

    stages {
        // Define stages
        stage('Deploy to Development') {
            agent {
                kubernetes {
                  yaml '''
                    apiVersion: v1
                    kind: Pod
                    spec:
                        serviceAccount: jenkins-agent
                        containers:
                        - name: helm-runner
                          image: alpine/k8s:1.27.13
                          command:
                          - cat
                          tty: true                       
                    '''
                }
            }
            steps {
                container('helm-runner') {
                    // Deploy to development environment using Helm
                    sh "helm ls -aA"
                    sh "helm upgrade --install ${CHART_NAME} ./${CHART_NAME} --namespace ${DEV_NAMESPACE}"
                }
            }
        }
    }
}