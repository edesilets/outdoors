#!/usr/bin/env groovy

pipeline {
    agent any
    stages {
        stage('Build') {
            steps { 
                echo 'Building..'
                echo 'Using Git..'
                checkout scm
                echo 'Composer Installing Packages..'
                sh "composer install"
                echo 'Copy example env to env'
                // need to copy from location that has the env stored.....
                sh 'cp .env.example .env'
                echo 'generate artisan project key'
                sh 'php artisan key:generate'
                echo "one last thing $WORKSPACE"
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
                sh './vendor/bin/phpunit'
            }
        }
        stage('Deploy') {
            steps {
                echo "Deploying.... ${params.PERSON}"
                // sh "ansible-playbook ./deploy/scripts/playbook.yml"
                ansiblePlaybook('deploy/scripts/playbook.yml') {
                    inventoryPath('hosts.ini')
                    ansibleName('Ansible 2.0.0.2')
                    // tags('one,two')
                    // credentialsId('credsid')
                    sudo(true)
                    sudoUser("root")
                    // extraVars {
                    //     extraVar("key1", "value1", false)
                    //     extraVar("key2", "value2", true)
                    // }
                }
            }
        }
    }
}