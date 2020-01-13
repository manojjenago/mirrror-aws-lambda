pipeline {
  agent any
  stages {
    stage('Build') {
      parallel {
        stage('server') {
          steps {
            sh '''echo " building the server code..."
mvn -version
mkdir -p target
touch "target/server.war"'''
            stash(name: 'server', includes: '**/*.war')
          }
        }

        stage('client') {
          steps {
            sh '''echo "Build the client..."
npm install --save react
mkdir -p dist
cat > dist/index.html <<EOF
hello!..
touch :dist/client.js"'''
          }
        }

      }
    }

  }
}