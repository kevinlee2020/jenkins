#tutorial
https://cloud.google.com/solutions/continuous-delivery-jenkins-kubernetes-engine

#sample Jenkins file:
https://github.com/GoogleCloudPlatform/continuous-deployment-on-kubernetes


#非容器化Jenkins连接Kubernetes
https://blog.csdn.net/mario_hao/article/details/81332546
https://blog.csdn.net/myy1066883508/article/details/106235504/?utm_medium=distribute.pc_relevant.none-task-blog-title-3&spm=1001.2101.3001.4242


# 学习jenkinsfile中连接k8s方法
http://jianshu.com/p/2d89fd1b4403?from=singlemessage
https://juejin.im/post/6844904158391173133

#采用jenkins pipeline实现自动构建并部署至k8s
https://blog.csdn.net/caohongshuang/article/details/90170471   ---参考价值大
https://github.com/devopssec/kubernetes/blob/master/Jenkinsfile ---参考价值大


#Docker安装Jenkins及自动部署Maven项目
https://blog.csdn.net/weixin_44249490/article/details/103687307?utm_medium=distribute.pc_relevant.none-task-blog-title-6&spm=1001.2101.3001.4242

#Docker+JenKins实现自动化部署
https://blog.csdn.net/qq_42766492/article/details/90760217?utm_medium=distribute.pc_relevant.none-task-blog-title-7&spm=1001.2101.3001.4242

base64 /root/.kube/config > kube-config-base64.txt
cat kube-config-base64.txt

#Jenkins项目配置Jenkinsfile + KubernetesPod.yaml
https://blog.csdn.net/sundehui01/article/details/108149843 ---参考价值大

# jenkinsfile， sh 'sed -i s/{{version}}/${BUILD_NUMBER}/g demo-maven-tomcat/icp/deploy.yaml'
http://songjxin.cn/?p=207

#how to catch branch name in jenkinsfile
https://stackoverflow.com/questions/51375267/how-to-catch-branch-name-in-jenkinsfile ---参考价值大




docker run \
  -d \
  -u root \
  -p 8080:8080 \
  -v jenkins_home:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  jenkinsci/blueocean

 http://localhost:8080

            steps {
                sh "mkdir -p ~/.kube"
                sh "echo ${K8S_CONFIG} | base64 -d > ~/.kube/config"
                sh "sed -e 's#{IMAGE_URL}#${params.HARBOR_HOST}/${params.DOCKER_IMAGE}#g;s#{IMAGE_TAG}#${GIT_TAG}#g;s#{APP_NAME}#${params.APP_NAME}#g;s#{SPRING_PROFILE}#k8s-test#g' k8s-deployment.tpl > k8s-deployment.yml"
                sh "kubectl apply -f k8s-deployment.yml --namespace=${params.K8S_NAMESPACE}"
            }
