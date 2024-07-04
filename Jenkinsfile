pipeline {
    agent any
    

    environment {
        // Define environment variables
        CHART_NAME = 'hexagun-webapp' // Name of the Helm chart
        DEV_NAMESPACE = 'dev' // Namespace for development environment        
        PROD_NAMESPACE = 'prod' // Namespace for prod environment
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
                    script {
                        if (env.BRANCH_NAME == 'main') {
                            // Deploy to prod environment using Helm
                            sh "helm ls -aA"
                            sh "helm upgrade --install ${CHART_NAME} ./${CHART_NAME} --namespace ${PROD_NAMESPACE}"
                        } else if (env.BRANCH_NAME =~ /^dev.*/ ) {
                            // Deploy to development environment using Helm
                            sh "helm ls -aA"
                            sh "helm upgrade --install ${CHART_NAME} ./${CHART_NAME} --namespace ${DEV_NAMESPACE}"
                        } else {
                            // Generate template
                            sh "helm ls -aA"
                            sh "helm template ./${CHART_NAME}"
                        }
                    }
                }
            }
        }
    }
}