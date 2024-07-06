
<a name="nW8VQ"></a>
### vardacico 

```bash
docker pull verdaccio/verdaccio:4
```
<br /> 
```bash
docker run -it --rm --name verdaccio -p 4873:4873 verdaccio/verdaccio
```
<br /> [https://verdaccio.org/docs/docker](https://verdaccio.org/docs/docker)
<a name="iM3RU"></a>
### <br />
<a name="xI1Lg"></a>
### 拷贝docker中的配置到宿主机

- 进入docker内部

```
docker exec -it verdaccio /bin/sh
```

- 进入到指定目录

```
 cd  /verdaccio
```
![1720247362272.png](https://cdn.nlark.com/yuque/0/2024/png/1392579/1720247370556-2584ff07-8377-4eb2-8869-e92f6bd4fffa.png#averageHue=%23264458&clientId=ud3bac816-fe1d-4&from=paste&height=82&id=uf56f7a8c&originHeight=102&originWidth=617&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=82877&status=done&style=none&taskId=uabd7ce5d-1f47-4902-8627-9bdec41499a&title=&width=493.6)<br />![1720249148837.png](https://cdn.nlark.com/yuque/0/2024/png/1392579/1720249156527-a481a93d-d133-404d-9a76-8d4845fa66fb.png#averageHue=%23000000&clientId=ud3bac816-fe1d-4&from=paste&height=52&id=u7923c91c&originHeight=65&originWidth=510&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=2385&status=done&style=none&taskId=u0286d927-d480-4f65-b126-6e7c554f1f3&title=&width=408)

-   开始拷贝到指定目录

```
 docker  cp verdaccio:/verdaccio /var/docker-data/verdaccio
```
![1720247824097.png](https://cdn.nlark.com/yuque/0/2024/png/1392579/1720247844341-a6a9af6b-b00e-4d07-a486-1b07b2c0d219.png#averageHue=%23213b4e&clientId=ud3bac816-fe1d-4&from=paste&height=67&id=u6ec6ce62&originHeight=84&originWidth=981&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=109056&status=done&style=none&taskId=u63281478-e256-470f-a4bf-02ee10174d1&title=&width=784.8)

- 停止并删除之前的容器
```
docker stop verdaccio
```
```
docker rm verdaccio
```

-   启动并绑定卷
```
docker run -it -d --name verdaccio -p 4873:4873 -v /var/docker-data/verdaccio/verdaccio/storage:/verdaccio/storage -v /var/docker-data/verdaccio/verdaccio/conf:/verdaccio/conf -v /var/docker-data/verdaccio/verdaccio/plugins:/verdaccio/plugins  verdaccio/verdaccio:4.0.0
```


<a name="sFUHE"></a>
### npm源管理器 nrc

- 安装nrc
```
npm install -g nrm
```

-  查看安装源
```
nrm ls
```
![1720246887570.png](https://cdn.nlark.com/yuque/0/2024/png/1392579/1720246893229-0513ff02-0ae2-421d-88e4-d3790689069e.png#averageHue=%23212121&clientId=ud3bac816-fe1d-4&from=paste&height=115&id=u35a86dde&originHeight=144&originWidth=669&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=13409&status=done&style=none&taskId=u13cffd18-59ae-41ab-b6b2-beb5d7ff5a2&title=&width=535.2)

<a name="dvGj3"></a>
### 给pakage包权限

```
chmod  777
```
