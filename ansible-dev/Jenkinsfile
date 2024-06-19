pipeline{
    agent any

    stages{
        stage("zip the file"){
            steps{
                sh 'rm -rf *.zip || echo ""'
                sh 'zip -r ansible/ansible-${BUILD_ID}.zip * --exclude Jenkinsfile'
                sh 'ls -l'
            }
        }
        
        stage('upload artifacts to jfrog'){
            steps{
                sh 'curl -uadmin:AP6Q75MQXFQHggFWzUUVfo8HfEy -T \
                ansible-${BUILD_ID}.zip \
                "http://18.214.87.232:8081/artifactory/ansible-doc/ansible-${BUILD_ID}.zip"'
                
            }
        }
        stage('publish to ansible server'){
            steps{
                sshPublisher(publishers: [sshPublisherDesc(configName: 'ansible-server', transfers: [sshTransfer(cleanRemote: false, \
                excludes: '', execCommand: 'unzip -o ansible-${BUILD_ID}.zip; rm -rf ansible-${BUILD_ID}.zip', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, \
                patternSeparator: '[, ]+', remoteDirectory: '/', remoteDirectorySDF: false, removePrefix: '', \
                sourceFiles: 'ansible-${BUILD_ID}.zip')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])

            }
        }
    }
}