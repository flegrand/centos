def PROJECT='centos'
def GIT_URL='git@github.com:flegrand/'+PROJECT+'.git'
def REGISTRY_URL='registry.demo.cloudcontrolled.net/demo/'+PROJECT

node {
        git branch: env.BRANCH_NAME, credentialsId: 'jenkins', url: GIT_URL

        stage "Build and Push Docker image"
        sh '. /var/jenkins_home/ucp-bundle/env.sh'
        withDockerRegistry(registry: [credentialsId: 'jenkins', url: "https://registry.demo.cloudcontrolled.net"]) {
                withDockerServer([credentialsId: "ucp", uri: "tcp://159.100.249.45:443"]) {
                        dockerImg = docker.build(REGISTRY_URL+':'+env.BRANCH_NAME+'-build'+env.BUILD_NUMBER,'.')
                        dockerImg.push()
                        dockerImg.push(env.BRANCH_NAME)
                }
        }

        // Launch dependant jobs
        build job: 'httpd/2.4', wait: false
        build job: 'java/openjdk-7-jdk', wait: false
        build job: 'java/openjdk-8-jdk', wait: false
}