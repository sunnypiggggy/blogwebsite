---
title: docker-compose优雅部署hexo
mathjax: true
date: 2022-06-04 17:31:18
tags:
comments: true
---

简单介绍下使用docker-compose搭建caddy2 + hexo 博客站点，利用Github的webhook功能，实现自动化部署。 

关键词：docker-compose caddy2 hexo


# 1. 组件
安装docker-compose不再赘述
直接上docker-compose.yml  caddyfile 和 Dockerfile

docker-compose.yml
```yml
version: "3"
services:
  caddy:
      restart: always
#      image: caddy:latest
      build:
          context: caddy
          dockerfile: Dockerfile
      volumes:
          - ./caddyfile:/etc/caddy/Caddyfile
          - ./data/:/data/
          - ./blog_site:/srv
      environment:
          - ACME_AGREE=true
      ports:
          - "80:80"
          - "443:443"
```

caddyfile
https://blog.sunnypiggy.xyz/webhook 输入到博客repo的webhook，请自定义密码。
```json

{
debug
  git {
    repo blogwebsite {
      base_dir /srv
      url https://github.com/sunnypiggggy/blogwebsite.git
      webhook Github X-Hub-Signature-256  {your_secert}
      branch master
    }
  }
}


blog.sunnypiggy.xyz
{
	route /version* {
		respond * "1.0.0" 200
	}
	route /webhook {
		git update repo blogwebsite
	}
	route {
		file_server {
			root  /srv/blogwebsite/public
		}
	}
    handle_errors {
		@404 {
			expression {http.error.status_code} == 404
		}
		rewrite @404 /404.html
		root * /srv/blogwebsite/public
		file_server
	}
}
```

Dockerfile
由于caddy默认的镜像不支持caddy-git，需要构建自定义镜像。
```dockerfile

FROM caddy:2.5.1-builder-alpine AS builder

RUN xcaddy build \
  --with github.com/caddyserver/transform-encoder \
  --with github.com/kirsch33/realip \
  --with github.com/greenpau/caddy-git \
  --with github.com/WingLim/caddy-webhook \
  --with github.com/mholt/caddy-webdav

FROM caddy:2.5.1-alpine

COPY --from=builder /usr/bin/caddy /usr/bin/caddy

CMD ["caddy", "run", "--environ", "--config", "/etc/caddy/Caddyfile", "--adapter", "caddyfile"]
```


# 2. Hexo 搭建

- 配置fluid主题，请按照[说明][1]。

[1]:https://hexo.fluid-dev.com/docs/guide/

- 公式一直让人困扰，要安装一大堆有的没得，对优雅部署方案一点都不友好。好在找到了不错的方案。

```shell
npm i hexo-filter-mathjax --save
npm i hexo-math --save
hexo clean
```

把以下内容添加到 <Hexo>/_config.yml 文件：
```yaml
mathjax:
  tags: none # 或 'ams' 或 'all'
  single_dollars: true # 启用单个美元符号作为内联（行内）数学公式定界符
  cjk_width: 0.9 # 相对 CJK 字符宽度
  normal_width: 0.6 # 相对正常（等宽）宽度
  append_css: true # 将 CSS 添加到每个页面
  every_page: false # 如果为 true，那么无论每篇文章的前题中的 `mathjax` 设置如何，每页都将由 mathjax 呈现
```
在需要的博文title上添加：
```shell
---
title: Docker
mathjax: true
date: 2022-06-04 17:31:18
tags:
---
```
实例：

$$
E=mc^2
$$


