# k8s 部署 jmeter
### 1，部署grafana

```
cd dspt/grafana

sh deployGrafana.sh
```

### 2, 部署influxdb

```
cd dspt/test
kubectl apply -f ServiceAccount.yaml
kubectl apply -f Service.yaml
# 等待 1分钟左右
kubectl apply -f job.yaml
kubectl apply -f DaemonSet.yaml
```

结果

```
[root@master test]# kubectl get pods -n monitoring
NAME                   READY   STATUS      RESTARTS   AGE
influxdb-0             1/1     Running     0          18h
influxdb-setup-khd4j   0/1     Completed   0          18h
telegraf-f8rrv         1/1     Running     0          18h
telegraf-td7sd         1/1     Running     0          18h
```

3, Grafana 配置 influxdb数据源

![image](https://github.com/Mountains-and-rivers/k8s-jmeter/blob/main/images/1.png)

说明

```
1，选择Flux Query， Fluxdb 2.x 只支持Flux 查询模式
2，URL 配置  # Fluxdb beta 2.x 版本使用9999 端口作为http api 访问端口，这里映射为 30002 外部访问
3，Origanization 为启动influxdb 配置
4， Token 配置 # 2.x 要配置token
 Authentication: Token ayK1YVVKiTnBPL4PTzjSMwOfrN6mgWUZK6XV05xlOgrViC-IB7W9xxSQfl9seinUwDSowZUiqUBjEuBrqvGdRA==
5，Default Bucket # 默认查询数据库 启动时创建
```

![image](https://github.com/Mountains-and-rivers/k8s-jmeter/blob/main/images/2.png)

结果：

网上找了个模板，实际要自己创建

![image](https://github.com/Mountains-and-rivers/k8s-jmeter/blob/main/images/3.png)

参考官网

![image](https://github.com/Mountains-and-rivers/k8s-jmeter/blob/main/images/4.png)

https://docs.influxdata.com/influxdb/v2.1/visualize-data/dashboards/

## 3, 部署jmeter

jdk 11 +

![image](https://github.com/Mountains-and-rivers/k8s-jmeter/blob/main/images/5.png)

![image](https://github.com/Mountains-and-rivers/k8s-jmeter/blob/main/images/6.png)

![image](https://github.com/Mountains-and-rivers/k8s-jmeter/blob/main/images/7.png)

![image](https://github.com/Mountains-and-rivers/k8s-jmeter/blob/main/images/8.png)



## 4，导出报告

```
jmeter.bat -Jjmeter.save.saveservice.output_format=xml -n -t HTTP请求.jmx -l Test.jtl

JMeterPluginsCMD.bat --generate-csv D:\apache-jmeter-5.4.3\bin\result.csv  --input-jtl D:\apache-jmeter-5.4.3\bin\Test.jtl  --plugin-type AggregateReport
```

压力测试情况下，无法自动化，主要因为没有高可用inflxudb集群。

参考

https://blog.csdn.net/lzf9651/article/details/116129354

https://blog.csdn.net/zuozewei/article/details/115299107

https://github.com/zuozewei/blog-example/tree/master/Kubernetes/k8s-jmeter-cluster

https://github.com/mderevyankoaqa/jmeter-influxdb2-listener-plugin
