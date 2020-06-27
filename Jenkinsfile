#!groovy
def deploy_branch_stag = "staging"
def deploy_branch_prod = "master"
def deploy_project_stag = "user1-app-stag"
def deploy_project_prod = "user1-app-prod"

pipeline {
    agent {
        kubernetes {
            cloud 'openshift'
            yamlFile 'jenkins-agent-pod.yaml'
        }
    }

    stages{
        stage('staging deploy'){
            when {
                expression {
                    return env.GIT_BRANCH == "${deploy_branch_stag}" || params.FORCE_FULL_BUILD
                }
            }
            steps{
                echo "staging deploy"
                openshift.withCluster() {
                    openshift.withProject("${deploy_project_stag}") {
                        openshift.apply(openshift.process('-f', 'template-deploy.yaml'))
                    }
                }
            }
        }

        stage('staging deploy'){
            when {
                expression {
                    return env.GIT_BRANCH == "${deploy_branch_prod}" || params.FORCE_FULL_BUILD
                }
            }
            steps{
                echo "production deploy"
                openshift.withCluster() {
                    openshift.withProject("${deploy_project_prod}") {
                        openshift.apply(openshift.process('-f', 'template-deploy.yaml'))
                    }
                }
            }
        }
    }
}
