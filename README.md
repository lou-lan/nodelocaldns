1. 加载新镜像安装包到所有节点
2. 卸载旧的 node-local-dns
  ```bash
  kubectl get ds -n kube-system
  kubectl -n kube-system delete ds node-local-dns
  ```
3. 应用新的
  ```bash
  kubectl apply -f test-new.yml
  ```
4. 检查是否全部启动
  ```bash
  kubectl get pods -n kube-system | grep node-local-dns
  ```
5. 查看其中一个的 node-local-dns 的日志
  ```bash
  kubectl logs node-local-dns-77hww -n kube-system -f --tail 100
  ```
6. 等待日志输出 `Added back nodelocaldns rule` 相关日志
  ```bash
  [INFO] Added back nodelocaldns rule - {raw PREROUTING [-p tcp -d 10.96.0.10 --dport 53 -j NOTRACK -m comment --comment NodeLocal DNS Cache: skip conntrack]}
  [INFO] Added back nodelocaldns rule - {raw PREROUTING [-p udp -d 10.96.0.10 --dport 53 -j NOTRACK -m comment --comment NodeLocal DNS Cache: skip conntrack]}
  [INFO] Added back nodelocaldns rule - {filter INPUT [-p tcp -d 10.96.0.10 --dport 53 -j ACCEPT -m comment --comment NodeLocal DNS Cache: allow DNS traffic]}
  [INFO] Added back nodelocaldns rule - {filter INPUT [-p udp -d 10.96.0.10 --dport 53 -j ACCEPT -m comment --comment NodeLocal DNS Cache: allow DNS traffic]}
  [INFO] Added back nodelocaldns rule - {raw OUTPUT [-p tcp -s 10.96.0.10 --sport 53 -j NOTRACK -m comment --comment NodeLocal DNS Cache: skip conntrack]}
  [INFO] Added back nodelocaldns rule - {raw OUTPUT [-p udp -s 10.96.0.10 --sport 53 -j NOTRACK -m comment --comment NodeLocal DNS Cache: skip conntrack]}
  [INFO] Added back nodelocaldns rule - {filter OUTPUT [-p tcp -s 10.96.0.10 --sport 53 -j ACCEPT -m comment --comment NodeLocal DNS Cache: allow DNS traffic]}
  [INFO] Added back nodelocaldns rule - {filter OUTPUT [-p udp -s 10.96.0.10 --sport 53 -j ACCEPT -m comment --comment NodeLocal DNS Cache: allow DNS traffic]}
  [INFO] Added back nodelocaldns rule - {raw OUTPUT [-p tcp -d 10.96.0.10 --dport 53 -j NOTRACK -m comment --comment NodeLocal DNS Cache: skip conntrack]}
  [INFO] Added back nodelocaldns rule - {raw OUTPUT [-p udp -d 10.96.0.10 --dport 53 -j NOTRACK -m comment --comment NodeLocal DNS Cache: skip conntrack]}
  [INFO] Added back nodelocaldns rule - {raw OUTPUT [-p tcp -d 10.96.0.10 --dport 8080 -j NOTRACK -m comment --comment NodeLocal DNS Cache: skip conntrack]}
  [INFO] Added back nodelocaldns rule - {raw OUTPUT [-p tcp -s 10.96.0.10 --sport 8080 -j NOTRACK -m comment --comment NodeLocal DNS Cache: skip conntrack]}
  ```
8. 在该 node-local-dns pod 的节点的业务 pod 发起 dns 请求
