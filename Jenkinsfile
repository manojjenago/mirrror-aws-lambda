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

    stage('Unit/Smoke-Testing') {
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

    stage('E2E-Testing-TST') {
      parallel {
        stage('TST04') {
          steps {
            echo 'TST04-Testing'
          }
        }

        stage('TST05') {
          steps {
            echo 'TST05'
          }
        }

      }
    }

    stage('UAT') {
      steps {
        echo 'UAT Validation'
      }
    }

    stage('Report') {
      steps {
        echo 'Publish Report'
        input(message: 'IS QA Passed', ok: 'GO ahead for Deploy', submitter: 'mjena')
      }
    }

    stage('Deploy') {
      steps {
        unstash 'server'
        unstash 'client'
        sh '''APP_DIR=/usr/local/tomcat/webapps
rm -rf $APP_DIR/ROOT
cp target/server.war $APP_DIR/server.war
mkdir -p $APP_DIR/ROOT
cp dist/* $APP_DIR/ROOT
/usr/local/tomcat/bin/startup.sh'''
      }
    }

  }
}