Quartz 随附了一个 Docker 镜像，可以让你在本地预览 Quartz，而无需安装 Node。

你可以运行下面的一行命令，在 Docker 中运行 Quartz。

```sh
docker run --rm -itp 8080:8080 -p 3001:3001 -v ./content:/usr/src/app/content $(docker build -q .)
```
