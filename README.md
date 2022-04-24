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
```

## 3, 部署jmeter



