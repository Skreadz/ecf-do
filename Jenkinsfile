pipeline {
    agent {
        label 'ansible-it-cloud' // Agent Jenkins ciblé pour l’exécution
    }
    options {
        ansiColor('xterm') // Affichage coloré des logs
        buildDiscarder(logRotator(numToKeepStr: '5')) // Conservation des 5 derniers builds
    }
    environment {
        ANSIBLE_DISPLAY_SKIPPED_HOSTS = 'true'
        ANSIBLE_DISPLAY_OK_HOSTS = 'true'
    }
    parameters {
        // Paramètres pour activer/désactiver chaque déploiement
        booleanParam(name: 'DEPLOY_KUBE_MASTER', defaultValue: false, description: 'Déployer le master Kubernetes')
        booleanParam(name: 'DEPLOY_OPENFAAS', defaultValue: false, description: 'Déployer OpenFaaS')
        booleanParam(name: 'DEPLOY_SPRING_APP', defaultValue: false, description: 'Déployer Srping App')
        booleanParam(name: 'DEPLOY_ANGULAR_APP', defaultValue: false, description: 'Déployer Angular App')
        booleanParam(name: 'DEPLOY_ELK_STACK', defaultValue: false, description: 'Déployer Elk Stack')
        // Ajoutez ici d’autres paramètres si besoin
    }
    stages {
        stage("install ansible collections") {
            when {
                expression { params.DEPLOY_OPENFAAS }
            }
            steps {
                // Installation des collections Ansible nécessaires
                sh 'ansible-galaxy collection install -r requirements.yml'
            }
        }
        stage("deploy kube master") {
            when {
                expression { params.DEPLOY_KUBE_MASTER }
            }
            steps {
                // Déploiement du master Kubernetes via playbook Ansible
                ansiblePlaybook(
                    colorized: true,
                    disableHostKeyChecking: true,
                    inventory: "envs/all/00_inventory.yml",
                    playbook: 'deploy-kube.yml',
                    vaultTmpPath: ''
                )
            }
        }
        stage("deploy openfaas") {
            when {
                expression { params.DEPLOY_OPENFAAS }
            }
            steps {
                // Déploiement d’OpenFaaS via playbook Ansible
                ansiblePlaybook(
                    colorized: true,
                    disableHostKeyChecking: true,
                    inventory: "envs/all/00_inventory.yml",
                    playbook: 'deploy-openfaas.yml',
                    vaultTmpPath: ''
                )
            }
        }
        stage("deploy spring-app") {
            when {
                expression { params.DEPLOY_SPRING_APP }
            }
            steps {
                // Déploiement de l’application Spring Boot via playbook Ansible
                ansiblePlaybook(
                    colorized: true,
                    disableHostKeyChecking: true,
                    inventory: "envs/all/00_inventory.yml",
                    playbook: 'deploy-spring-app.yml',
                    vaultTmpPath: ''
                )
            }
        }
        stage("deploy angular-app") {
            when {
                expression { params.DEPLOY_ANGULAR_APP }
            }
            steps {
                // Déploiement de l’application Angular via playbook Ansible
                ansiblePlaybook(
                    colorized: true,
                    disableHostKeyChecking: true,
                    inventory: "envs/all/00_inventory.yml",
                    playbook: 'deploy-angular-app.yml',
                    vaultTmpPath: ''
                )
            }
        }
        // Ajoutez ici d’autres stages conditionnés par d’autres paramètres si besoin
    }
    post {
        always {
            cleanWs() // Nettoyage du workspace Jenkins après chaque build
        }
    }
}