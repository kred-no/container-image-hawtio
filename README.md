# container-image-hawtio
![Build Status](https://github.com/kred-no/container-image-hawtio/actions/workflows/tomcat-build.yml/badge.svg?branch=main)

This repository contains a Docker image for running [Hawtio](https://hawt.io/), a web-based management console for Java applications.

## Usage

Run the Hawtio container image with default config.
Access Hawtio at [http://localhost:8080/hawtio-default](http://localhost:8080/hawtio-default).

```bash
# Run from registry image
docker run --rm -it -p 8080:8080 docker.io/kdsda/hawtio:latest
```

```bash
# Run docker compose example (./test/)
# Builds AciveMQ-Classic & hawtio instances & connects via jolokia endpoint.
docker compose -f compose up
```

## Variables

| Name | Default |
| --:  | :--     |
| CATALINA_OPTS     | `-Dhawtio.authenticationEnabled=false -Dhawtio.proxyAllowlist=*` |
| JAVA_TOOL_OPTIONS | `-XX:+ExitOnOutOfMemoryError -XX:InitialRAMPercentage=50.0 -XX:MaxRAMPercentage=75.0 -XX:+UseG1GC -XX:+UseStringDeduplication -XX:MaxGCPauseMillis=500 -XX:+PerfDisableSharedMem -Xss512k -Xshare:on -Xlog:gc*:stdout:uptime,level,tags` |

## License

This project is licensed under the MIT License.
