# Sentinel

### Homepage:

{% embed url="https://github.com/alibaba/Sentinel" %}



### Dockerfile

```
FROM openjdk:8-jre-alpine

ENV SENTINEL_HOME=/opt/sentinel
ENV SENTINEL_DASHBOARD_VERSION=1.8.6

RUN mkdir /opt/sentinel 
RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

COPY sentinel-dashboard-${SENTINEL_DASHBOARD_VERSION}.jar ${SENTINEL_HOME}/sentinel-dashboard-${SENTINEL_DASHBOARD_VERSION}.jar
 
WORKDIR ${SENTINEL_HOME}

EXPOSE 8080
EXPOSE 8719

ENTRYPOINT java -Djava.security.egd=file:/dev/./urandom -Dserver.port=8080 -Dcsp.sentinel.api.port=8719 -Dcsp.sentinel.dashboard.server=localhost:8080 -Dproject.name=sentinel-dashboard -jar sentinel-dashboard-${SENTINEL_DASHBOARD_VERSION}.jar
```

####

### 使用

#### how to download

```bash
docker pull willsrepo/sentinel:1.8.6
```

#### how to start

```bash
docker run --name sentinel  -d -p 8080:8080 -p 8719:8719 -d willsrepo/sentinel:1.8.6
```

#### how to login web-dashboard

```
visit: http://localhost:8080/

account and password: [sentinel sentinel]

just enjoy :)
```

