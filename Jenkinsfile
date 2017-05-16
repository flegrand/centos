def PROJECT='centos'
def GIT_URL='http://flegrand@git.demo.cloudcontrolled.net:8800/demo/'+PROJECT+'.git'

node {
        git branch: env.BRANCH_NAME, credentialsId: 'flegrand', url: GIT_URL

        stage "Build and Push Docker image"
        withDockerRegistry(registry: [credentialsId: 'flegrand']) {
                withDockerServer(server: [uri: 'unix:///var/run/docker.sock']) {
                        dockerImg = docker.build('flegrand/'+PROJECT+':'+env.BRANCH_NAME+'-build'+env.BUILD_NUMBER,'.')
                        dockerImg.push()
                        dockerImg.push(env.BRANCH_NAME)
                }
        }

        // Launch dependant jobs
        build job: 'httpd/2.4', wait: false
}