pipeline {
    // pipelineを実行するagentの設定
    agent {
        kubernetes {
            cloud 'openshift'
            yamlFile 'jenkins-agent-pod.yaml'
        }
    }

    environment {
        deploy_project_stag = "user1-app-stag"
        deploy_project_prod = "user1-app-prod"
    }

    stages {
        stage('staging deploy') {
            when {
                // Gitのブランチ名を変数として取得して条件式に使う
                expression {
                    return env.GIT_BRANCH == "origin/staging" || params.FORCE_FULL_BUILD
                }
            }
            steps {
                echo "staging deploy"
                script {
                    openshift.withCluster() {
                        openshift.withProject("${deploy_project_stag}") {
                            // Apply application manifests
                            // sh "oc process -f template-deploy.yaml | oc apply -n ${deploy_project_stag} -f -"
                            openshift.apply(openshift.process('-f', 'template-deploy.yaml'))
                        }
                    }
                }
            }
        }

        stage('production deploy') {
            when {
                // Gitのブランチ名を変数として取得して条件式に使う
                expression {
                    return env.GIT_BRANCH == "origin/master" || params.FORCE_FULL_BUILD
                }
            }
            steps {
                echo "production deploy"
                script {
                    openshift.withCluster() {
                        openshift.withProject("${deploy_project_prod}") {
                            // Apply application manifests
                            // sh "oc process -f template-deploy.yaml | oc apply -n ${deploy_project_prod} -f -"
                            openshift.apply(openshift.process('-f', 'template-deploy.yaml'))
                        }
                    }
                }
            }
        }
    }
}
