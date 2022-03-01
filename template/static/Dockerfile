from openfaas/of-watchdog:0.8.2 AS watchdog
from alpine:3.15 AS build

COPY --from=watchdog /fwatchdog /usr/bin/fwatchdog
RUN chmod +x /usr/bin/fwatchdog

RUN apk update && apk add py-pygments ca-certificates bash git && rm -rf /var/cache/apk/*

WORKDIR /home/app
COPY ./function .

FROM alpine:3.15 AS runtime

WORKDIR /home/app

COPY --from=build /home/app/public public
COPY --from=build /usr/bin/fwatchdog .

ENV mode="static"
ENV static_path="/home/app/public"

HEALTHCHECK --interval=3s CMD [ -e /tmp/.lock ] || exit 1

CMD ["./fwatchdog"]

