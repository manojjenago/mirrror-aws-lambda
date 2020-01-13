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
            stash(name: 'client', includes: '**/dist/*')
          }
        }

      }
    }

    stage('UnitTesting') {
      parallel {
        stage('UnitTesting') {
          steps {
            build 'UnitTesting'
            svn(url: 'http://svn.gspt.net/rit/base/eBayProject/branches/E2E_102016', poll: true)
            mail(subject: 'UnitTestingResults', body: 'Unit testing results for run', from: 'mjena@radial.com', to: 'mjena@radial.com')
          }
        }

        stage('SmokeTesting') {
          steps {
            echo 'Smoke Testing'
          }
        }

      }
    }

  }
}