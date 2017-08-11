def PROJECT='centos'
def GIT_URL='git@github.com:flegrand/'+PROJECT+'.git'
def REGISTRY_ADDRESS='registry.demo.cloudcontrolled.net'
def REPOSITORY_PATH=REGISTRY_ADDRESS+'/demo/'+PROJECT
def DOCKER_SERVER_ENDPOINT='159.100.249.45:443'

node {
        git branch: env.BRANCH_NAME, credentialsId: 'jenkins', url: GIT_URL

        stage "Build and Push Docker image"
        withDockerRegistry(registry: [credentialsId: 'jenkins', url: "https://"+REGISTRY_ADDRESS]) {
                withDockerServer([credentialsId: "ucp", uri: "tcp://"+DOCKER_SERVER_ENDPOINT]) {
                        dockerImg = docker.build(REPOSITORY_PATH+':'+env.BRANCH_NAME+'-build'+env.BUILD_NUMBER,'.')
                        dockerImg.push()
                        dockerImg.push(env.BRANCH_NAME)
                }
        }

        // Launch dependant jobs
        build job: 'httpd/2.4', wait: false
        build job: 'java/openjdk-7-jdk', wait: false
        build job: 'java/openjdk-8-jdk', wait: false
}