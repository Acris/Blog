pipeline {
  agent any
  stages {
    stage('Initialize') {
      steps {
        dir(path: 'source') {
          git(url: 'git@gitlab.com:Acris/blog-source.git', branch: 'master', changelog: true, poll: true, credentialsId: 'archlinux')
        }
        
        dir(path: 'themes/next') {
          git(url: 'git@github.com:Acris/hexo-theme-next.git', branch: 'master', changelog: true, poll: true, credentialsId: 'archlinux')
        }
        
        echo 'Initialize success!'
      }
    }
    stage('Build') {
      steps {
        sh 'npm install hexo-cli gulp-cli -g && npm install && hexo clean && hexo generate && gulp'
        echo 'Build success!'
      }
    }
    stage('Deploy') {
      steps {
        sh 'rm -rf $BLOG_HOME/* && cp -a $WORKSPACE/public/. $BLOG_HOME'
        echo 'Deploy success!'
      }
    }
    stage('Clean') {
      steps {
        deleteDir()
        echo 'Clean success!'
      }
    }
  }
  environment {
    BLOG_HOME = '/usr/share/nginx/html/blog'
  }
  triggers {
    pollSCM('*/5 * * * *')
  }
}
