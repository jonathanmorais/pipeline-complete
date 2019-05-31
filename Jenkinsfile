MAJOR_VERSION="1.0"
IMAGE_NAME="python-frontend"
BUILD_FRONT = frontend/Dockerfile
BUILD_BACK = backend/Dockerfile

VERSION = (env.BRANCH_NAME != 'master') ? "${env.BRANCH_NAME}-${MAJOR_VERSION}.${env.BUILD_ID}" : "${MAJOR_VERSION}.${env.BUILD_ID}"

node ('build') {
  try {
    stage "Install packages"
      checkout scm 
      def run_image = docker.build("${env.BUILD_BACK}", "${env.BUILD_FRONT}", "--network=host")
      
     // def run_image = docker.build("${env.BUILD_BACK}", "--network=host")
    // stage "Push"
    //   run_image.push()
    //   sh "docker rmi '${fullImageName(IMAGE_NAME)}'"

    stage "Notification and Cleanup"
      createGitTag()
      getDockerRunCommand([IMAGE_NAME])
      deleteDir()

  } catch(error) {
    deleteDir()
    throw error
  }
}

def createGitTag() {
  withCredentials([sshUserPrivateKey(credentialsId: 'github', keyFileVariable: 'GITHUB_KEY')]) {
    sh 'echo ssh -i $GITHUB_KEY -l git -o StrictHostKeyChecking=no \\"\\$@\\" > run_ssh.sh'
    sh 'chmod +x run_ssh.sh'
    withEnv(['GIT_SSH=./run_ssh.sh']) {
      sh "git tag '${VERSION}'"
      sh "git push origin '${VERSION}'"
    }
  }
}

// def fullImageName(imagePattern) {
//     return "docker-registry.stone.com.br:5000/fraud-detection/${imagePattern}:${VERSION}"
// }

// def getDockerRunCommand(imagePatternList) {
//     for (imagePattern in imagePatternList) {
//        sh """sed -i 's|<'"$imagePattern"'-image>|'"${fullImageName(imagePattern)}"'|g' Makefile.run"""
//     }
//     sh "cat Makefile.run"
// }