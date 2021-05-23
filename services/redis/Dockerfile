FROM redis:6.2-alpine

RUN sed -i "s/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g" /etc/apk/repositories \
  && apk --no-cache add tzdata \
  && cp "/usr/share/zoneinfo/Asia/Shanghai" /etc/localtime \
  && echo "Asia/Shanghai" > /etc/timezone