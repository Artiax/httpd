def name;
def tag;

node {
  stage('Prepare') {
    name = sh(returnStdout: true, script: 'basename -s .git $(git config --get remote.origin.url)')
    commit = sh(returnStdout: true, script: 'git rev-parse HEAD').trim().take(8)
    timestamp = sh(returnStdout: true, script: "date '+%Y-%m-%d-%H%M'").trim()
    tag = "${timestamp}-${commit}"

    git branch: 'master', url: 'https://github.com/Artiax/jenkins-jobs.git', changelog: false, poll: false
  }

  load 'flows/build'
}
