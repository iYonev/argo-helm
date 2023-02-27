node {
    def app

    stage('Clone repository') {
        checkout scm
    }

    stage('Update GIT') {
        script {
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                withCredentials([usernamePassword(credentialsId: 'git', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                    sh "git config user.email saintsinnerr@gmail.com"
                    sh "git config user.name iYonev"
                    sh "cat webapp-chart/values.yaml"
                    sh "sed -i 's+saintsinnerr/webapp.*+saintsinnerr/webapp:${DOCKERTAG}+g' webapp-chart/values.yaml"
                    sh "cat webapp-chart/values.yaml"
                    sh "git add ."
                    sh "git commit -m 'Done by Jenkins Job changemanifest: ${env.BUILD_NUMBER}'"
                    sh 'git rev-parse HEAD >> version.txt'
                    sh 'echo ${BUILD_TIMESTAMP} >> version.txt'
                    sh "git add version.txt"
                    sh "git commit -m 'Update version.txt to commit '"
                    sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/manifest2.git HEAD:main"
                }
            }
        }
    }
}
