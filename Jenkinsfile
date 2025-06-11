pipeline {
    agent any

	// Set the environment variables
    environment {
        AWS_REGION = 'us-east-1'
        CUSTOM_BIN = "${env.HOME}/bin"
        PATH = "${env.HOME}/bin:${env.PATH}"
    }

	// Multistage pipeline
    stages {
		// Stage 1 - Checkout code repository
        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    credentialsId: 'Github',
                    url: 'https://github.com/prashant-aggarwal/awr-devops-eks-cluster-destroy.git'
            }
        }

		// Stage 1 - Install kubectl
        stage('Install kubectl') {
            steps {
                sh '''
                echo "Installing kubectl..."
                curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.31.2/2024-11-15/bin/linux/amd64/kubectl
				chmod +x ./kubectl
				mkdir -p $HOME/bin
				cp ./kubectl $HOME/bin/kubectl
				export PATH=$HOME/bin:$PATH
				kubectl version --client
                '''
            }
        }

        // Stage 2 - Install eksctl
        stage('Install eksctl') {
            steps {
                sh '''
                echo "Installing eksctl..."
                export ARCH=amd64
                export PLATFORM=$(uname -s)_$ARCH
                curl -sLO "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_$PLATFORM.tar.gz"
                tar -xzf eksctl_$PLATFORM.tar.gz -C /tmp
                mkdir -p $HOME/bin
                mv /tmp/eksctl $HOME/bin/eksctl
                chmod +x $HOME/bin/eksctl
                export PATH=$HOME/bin:$PATH
                eksctl version
                '''
            }
        }
		
		// Stage 3 - Destroy EKS Cluster using cluster.yaml
        stage('Destroy EKS Cluster') {
            steps {
				script {
				// Install AWS Steps plugin to make this work
				withAWS(region: "${env.AWS_REGION}", credentials: 'AWS') {
						try {
							sh '''
							echo "Destroying EKS cluster from YAML..."
							eksctl delete cluster -f cluster.yaml
							'''
						} catch (exception) {
							echo "❌ Failed to destroy EKS cluster: ${exception}"
							error("Halting pipeline due to EKS cluster destruction failure.")
						}
					}
                }
            }
        }
    }

    // Cleanup the workspace in the end
	post {
        always {
            cleanWs()
        }
    }
}
