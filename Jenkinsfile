pipeline {
    agent any
    
    environment {
        PROJECT_NAME = 'Pipeline CI/CD Jenkins'
        DEPLOY_PATH = '/tmp/deploy-jenkins-app'
    }
    
    stages {
        stage('🔔 Préparation') {
            steps {
                echo '========================================='
                echo '🚀 Début du Pipeline CI/CD'
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
                    echo "Fichiers HTML et CSS détectés :"
                    du -h index.html style.css
                '''
                echo '✅ Build terminé avec succès !'
            }
        }
        
        stage('🚀 Déploiement') {
            steps {
                echo '🚀 Déploiement de l\'application...'
                sh """
                    # Création du répertoire de déploiement
                    mkdir -p ${env.DEPLOY_PATH}
                    
                    # Copie des fichiers
                    cp index.html ${env.DEPLOY_PATH}/
                    cp style.css ${env.DEPLOY_PATH}/
                    
                    echo "Fichiers déployés dans : ${env.DEPLOY_PATH}"
                    ls -la ${env.DEPLOY_PATH}
                """
                echo '✅ Déploiement réussi !'
            }
        }
        
        stage('✅ Vérification finale') {
            steps {
                echo '✅ Vérification du déploiement...'
                sh """
                    if [ -f "${env.DEPLOY_PATH}/index.html" ]; then
                        echo "✅ Application correctement déployée"
                        echo "📍 Chemin : ${env.DEPLOY_PATH}"
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
            echo "📦 Application déployée : ${env.DEPLOY_PATH}"
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