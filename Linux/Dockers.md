---
date: 2025-07-09
---
---

# 数据卷

<mark style="background: #ABF7F7A6;">数据卷(volume) </mark>是一个虚拟目录，是容器内目录与宿主机目录之间映射的桥梁。

以Nginx为例，Nginx中有两个关键的目录：

- `html`：放置一些静态资源
    
- `conf`：放置配置文件
    
如果我们要让Nginx代理我们的静态资源，最好是放到`html`目录；如果我们要修改Nginx的配置，最好是找到`conf`下的`nginx.conf`文件。

但是，容器运行的Nginx所有的文件都在<mark style="background: #D2B3FFA6;">容器内部</mark>。所以必须利用数据卷将两个目录与宿主机目录关联，方便操作
![[Pasted image 20250709170949.png]]
这样，容器内的 *conf* 和 *html* 目录就 与宿主机的 *conf* 和 *html* 目录关联起来，称为**挂载**

此时再操作宿主机的<mark style="background: #ADCCFFA6;">/var/lib/docker/volumes/html/\_data</mark>就是在操作容器中的 <mark style="background: #FFB8EBA6;">/usr/share/nginx/html</mark>


---
## 命令

| 命令                    | 含义         |
| --------------------- | ---------- |
| docker volume create  | 创建数据卷      |
| docker volume rm      | 删除指定数据卷    |
| docker volume inspect | 查看某个数据卷的详情 |
| docker volume prune   | 清除数据卷      |
|                       |            |
<mark style="background: #FFB86CA6;">注意</mark> ：挂载之后要切换终端访问 *docker exec -it nginx bash*  不然没进入容器中 ...

![[Pasted image 20250709192035.png]]

# 镜像

![[Pasted image 20250709200031.png]]

<mark style="background: #ABF7F7A6;">构建入口的时候需要</mark>

![[Pasted image 20250709200321.png]]
<mark style="background: #21D16EA6;">示例 Dockerfile</mark>
![[Pasted image 20250709200526.png]]

进阶省略

![[Pasted image 20250709200632.png]]

