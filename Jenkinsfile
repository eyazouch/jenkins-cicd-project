pipeline {
    agent any
    
    environment {
        PROJECT_NAME = 'Pipeline CI/CD Jenkins & Ansible'
        DEPLOY_PATH = '/tmp/deploy-jenkins-app'
    }
    
    stages {
        stage('🔔 Préparation') {
            steps {
                echo '========================================='
                echo '🚀 Début du Pipeline CI/CD avec Ansible'
                echo "📦 Projet : ${env.PROJECT_NAME}"
                echo "🕐 Date : ${new Date()}"
                echo '========================================='
            }
        }
        
        stage('📥 Clone du Repository') {
            steps {
                echo '📥 Clonage du projet depuis GitHub...'
                checkout scm
                echo '✅ Code récupéré avec succès !'
            }
        }
        
        stage('🔨 Build') {
            steps {
                echo '🔨 Préparation de l\'application...'
                sh '''
                    echo "Contenu du projet :"
                    ls -la
                    
                    echo ""
                    echo "Fichiers détectés :"
                    du -h index.html style.css deploy.yml
                '''
                echo '✅ Build terminé avec succès !'
            }
        }
        
        stage('🔍 Vérification Ansible') {
            steps {
                echo '🔍 Vérification de l\'installation Ansible...'
                sh '''
                    ansible --version
                    echo ""
                    echo "✅ Ansible est installé et fonctionnel"
                '''
            }
        }
        
        stage('🚀 Déploiement avec Ansible') {
            steps {
                echo '🚀 Déploiement de l\'application avec Ansible...'
                sh '''
                    echo "Exécution du playbook Ansible..."
                    ansible-playbook deploy.yml -v
                '''
                echo '✅ Déploiement Ansible terminé !'
            }
        }
        
        stage('✅ Vérification finale') {
            steps {
                echo '✅ Vérification du déploiement...'
                sh """
                    if [ -f "${env.DEPLOY_PATH}/index.html" ]; then
                        echo "✅ Application correctement déployée"
                        echo "📍 Chemin : ${env.DEPLOY_PATH}"
                        echo ""
                        echo "Fichiers déployés :"
                        ls -lh ${env.DEPLOY_PATH}
                    else
                        echo "❌ Erreur de déploiement"
                        exit 1
                    fi
                """
            }
        }
    }
    
    post {
        success {
            echo '========================================='
            echo '✅ PIPELINE TERMINÉ AVEC SUCCÈS !'
            echo "📦 Application déployée avec Ansible"
            echo "📍 Chemin : ${env.DEPLOY_PATH}"
            echo '========================================='
        }
        failure {
            echo '========================================='
            echo '❌ PIPELINE ÉCHOUÉ'
            echo 'Vérifiez les logs ci-dessus'
            echo '========================================='
        }
        always {
            echo '🧹 Nettoyage du workspace...'
            cleanWs()
        }
    }
}