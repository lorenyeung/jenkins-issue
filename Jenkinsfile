pipeline {
    agent any

    stages {
        stage ('Clone') {
            steps {
                git url: "https://github.com/lorenyeung/jenkins-issue", branch: 'main'
            }
        }

        stage ('Artifactory configuration') {
            steps {
                rtGradleDeployer (
                    id: "GRADLE_DEPLOYER",
                    serverId: "loren-7x",
                    repo: "libs-release-local"
                )

                rtGradleResolver (
                    id: "GRADLE_RESOLVER",
                    serverId: "loren-7x",
                    repo: "libs-release"
                )
            }
        }

        stage ('Exec Gradle') {
            steps {
                rtGradleRun (
                    tasks: 'clean artifactoryPublish',
                    deployerId: "GRADLE_DEPLOYER",
                    useWrapper: true,
                    resolverId: "GRADLE_RESOLVER",
                )
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
