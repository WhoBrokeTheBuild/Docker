
pipeline {
    agent any
    
    options {
        skipDefaultCheckout()
        timeout(time: 1, unit: 'HOURS')
    }
    triggers {
        // Every Sunday
        cron("H 0 * * 0")
    }

    stages {

        stage('Setup') {
            steps {
                sh 'printenv'

                cleanWs disableDeferredWipeout: true, deleteDirs: true
            }
        }

        stage('Builders') {
            steps {
                checkout scm;

                script {
                    
                    def OSList = [];
                    findFiles(glob: "builder/*/Dockerfile").each {
                        file -> OSList.add(file.directory);
                    }

                    parallel OSList.collectEntries {

                        OS -> [ "${OS} Build & Push": {
                            stage("${OS} Build & Push") {

                                stage("${OS} Build") {
                                    echo "Building ${OS}"
                                }

                                stage("${OS} Push") {
                                    echo "Pushing ${OS}"
                                }
                            }
                        }]
                    }
                }
            }
        }
    }
}

