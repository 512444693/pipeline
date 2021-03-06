# pipeline

该pipeline的流程如下：
1. 创建镜像
2. 推送镜像到本地仓库
3. 在集群中运行

## 搭建仓库
```
kubectl create -f registry-with-ui
```

## 搭建jenkins

#### 创建镜像
```
docker build -t localhost:30000/zhangmin/jenkins -f jenkins/Dockerfile jenkins
```

#### 运行
```
kubectl create -f jenkins
```

#### 初始化

到集群中任何节点 http://node-ip:30002 访问jenkins

1. 由于jenkins检查是否联网是通过连接google，所以需要配置代理，下载推荐的插件
2. 插件下载完成后，在 插件管理--高级 里将代理删除

## 创建pipeline

1. 创建```Pipeline```项目
![image](https://raw.githubusercontent.com/512444693/resources/master/pipeline/1.png)
2. 配置
![image](https://raw.githubusercontent.com/512444693/resources/master/pipeline/2.png)
3. 打开```Blue Ocean```

![image](https://raw.githubusercontent.com/512444693/resources/master/pipeline/3.png)

4. 运行
![image](https://raw.githubusercontent.com/512444693/resources/master/pipeline/4.png)
 
## 其它
 
如果需要在gitlab中添加webhook，以达到自动触发构建的目的

1. 在jenkins中下载GitLab Plugin插件
2. 配置job的Build Triggers，选择```Build when a change is pushed to GitLab. GitLab CI Service URL: http://JENKINS-SERVER/project/JOB-NAME```
3. 修改jenkins配置，Configure Global Security -- 访问控制 -- 授权策略 -- 任何用户可以做任何事
4. 在gitlab项目中，Settings -- Integrations 中添加webhook，http://JENKINS-SERVER/project/JOB-NAME
