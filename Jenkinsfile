
pipeline {
    agent none

    environment {
        PROJECT_ZONE = "${JENK_INT_IT_ZONE}"
        PROJECT_ID = "${JENK_INT_IT_PROJECT_ID}"
        STAGING_CLUSTER = "${JENK_INT_IT_STAGING}"
        PROD_CLUSTER = "${JENK_INT_IT_PROD}"
        BUILD_CONTEXT_BUCKET = "${JENK_INT_IT_BUCKET}"
        BUILD_CONTEXT = "build-context-${BUILD_ID}.tar.gz"
        APP_NAME = "python-sample-gke"
        GCR_IMAGE = "gcr.io/${PROJECT_ID}/${APP_NAME}:${BUILD_ID}"
    }

    stages {
		stage("deploy") {
			agent {
				kubernetes {
					cloud 'kubernetes'
					label 'cd'
					yamlFile 'jenkins/python-cd.yaml'
				}
			}

			steps {
				container('cd') {
					dir("helm") {
						sh "echo 'Simple kubectl install!'"
						sh "curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl"
						sh "chmod +x ./kubectl"
						sh "mv ./kubectl /usr/local/bin/kubectl"

						sh "echo 'Simple helm install!'"
						sh "wget https://get.helm.sh/helm-v3.2.4-linux-amd64.tar.gz"
						sh "tar zxfv helm-v3.2.4-linux-amd64.tar.gz"
						sh "cp linux-amd64/helm ."

						sh "echo 'upgrade app!'"
						sh "./helm upgrade --install --wait app ./app"
					}
				}
			}
		}
    }
}
