pipeline {
    agent { //这里使用docker镜像来启动maven,这样有个好处就是多个工程同时构建时不会出现冲突而失败
        docker {
            image 'maven:3.6-alpine' 
            args '-u root -v /home/jenkins/mvnrepo:/root/.m2'  //持载到本地，减少重复下载量，使用ali源
        }
    }
    stages {
        stage('Pull Git Demo') {
            steps{
                //清理工作空间
                cleanWs();
                //拉取代码
                checkout([$class: 'GitSCM', branches: [[name: '*/docker-maven-plugin-2']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'demo']], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/hellxz/SpringBoot-DockerDemo.git']]])
            }
        }
        stage('Build') { 
            steps {
                dir('demo') { //切换目录到demo
                    //执行构建镜像命令，这里起作用的是maven的插件
                    //可以参考https://github.com/hellxz/SpringBoot-DockerDemo.git的使用方法，在docker-maven-plugin-2分支
                    sh 'mvn -B -DskipTests clean package'  
                }
            }
        }
    }
    post { //这里定义的是后置处理
        success {
            // 构建成功
            echo '构建完成，正在清理工作空间'
            cleanWs();
            echo '清理工作空间完成'
        }
        failure {
            // 构建失败，这里使用sh是因为echo不支持使用参数
            sh 'echo "构建失败，详情请查看$WORKSPACE"'
        }
        aborted {
            // 构建被中止
            echo '构建被中止'
        }
    }


}