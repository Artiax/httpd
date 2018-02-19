def image;
def commit;

node("docker") {
  stage("Clone") {
    git branch: 'master', url: "https://github.com/Artiax/httpd.git", changelog: false, poll: false
    commit = sh(returnStdout: true, script: "git rev-parse HEAD").trim().take(8)
  }

  stage("Build") {
    sh "sed -ri 's/COMMIT_ID/${commit}/' index.html"
    image = docker.build 'httpd'
  }

  stage("Tag") {
    timestamp = sh(returnStdout: true, script: "date '+%Y-%m-%d-%H%M'").trim()
    commit = sh(returnStdout: true, script: "git rev-parse HEAD").trim().take(8)
    tag = "${timestamp}-${commit}"

    image.tag "${tag}"
    image.tag "latest"
  }
}

timeout(time: 15, unit: 'MINUTES') {
  try {
    input(id: 'deploy', message: 'Deploy this image?')
    build job: 'deployments/httpd', parameters: [string(name: 'RELEASE_NAME', value: 'jenkins'),string(name: 'TAG', value: "${tag}")], wait: false
  } catch(err) {
    currentBuild.result = 'SUCCESS'
  }
}
