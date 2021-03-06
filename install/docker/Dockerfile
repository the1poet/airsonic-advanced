FROM adoptopenjdk/openjdk11:alpine-jre

LABEL description="Airsonic-Advanced is a free, web-based media streamer, providing ubiquitous access to your music." \
      url="https://github.com/airsonic-advanced/airsonic-advanced"

ENV AIRSONIC_PORT=4040 AIRSONIC_DIR=/var CONTEXT_PATH=/ UPNP_PORT=4041

WORKDIR $AIRSONIC_DIR

RUN apk --no-cache add \
    ffmpeg \
    lame \
    bash \
    libressl \
    fontconfig \
    ttf-dejavu \
    ca-certificates \
    tini

COPY run.sh /usr/local/bin/run.sh

RUN chmod +x /usr/local/bin/run.sh

COPY target/dependency/airsonic-main.war airsonic.war

EXPOSE $AIRSONIC_PORT

# Default DLNA/UPnP ports
EXPOSE $UPNP_PORT
EXPOSE 1900/udp

VOLUME $AIRSONIC_DIR/airsonic $AIRSONIC_DIR/music $AIRSONIC_DIR/playlists $AIRSONIC_DIR/podcasts

HEALTHCHECK --interval=15s --timeout=3s CMD wget -q http://localhost:"$AIRSONIC_PORT""$CONTEXT_PATH"rest/ping -O /dev/null || exit 1

ENTRYPOINT ["tini", "--", "run.sh"]
