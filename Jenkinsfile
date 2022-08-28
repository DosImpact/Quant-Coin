node {
   def commit_id
   environment { 
   }
   stage('Preparation') {
     checkout scm
     sh "git rev-parse --short HEAD > .git/commit-id"                        
     commit_id = readFile('.git/commit-id').trim()
     echo "${SERVER_URI_PANDAS}"
   }
//    stage('test') {
//      nodejs(nodeJSInstallationName: 'NodeJS') {
//        sh 'npm install --only=dev'
//        sh 'npm test'
//      }
//    }
   // https://www.jenkins.io/doc/book/pipeline/docker/#building-containers
   stage('Pandas docker build/push') {
     docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
       def dockerfile = 'Dockerfile.Pandas'
       def app = docker.build("ehdudtkatka/quant-coin-pandas:${commit_id}",  "-f ${dockerfile} ./").push()
     }
   }
   stage('Web docker build/push') {
     docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
       def dockerfile = 'Dockerfile.Web'
       def app = docker.build("ehdudtkatka/quant-coin-web:${commit_id}",  "--build-arg ${SERVER_URI_PANDAS} -f ${dockerfile} ./").push()
     }
   }
}