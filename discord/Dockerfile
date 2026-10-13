FROM debian:trixie-slim

ENV LANG=C.UTF-8 \
    LC_ALL=C.UTF-8 \
    XDG_RUNTIME_DIR=/run/user/1000

RUN useradd --uid 1000 --user-group --create-home discord \
    && install -d -m 0700 -o discord -g discord /run/user/1000

RUN apt-get update \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
        ca-certificates curl dbus-daemon libpulse0 libegl1 libegl-mesa0 libgl1-mesa-dri \
    && curl -fsSL "https://discord.com/api/downloads/distributions/app/installers/latest?channel=stable&platform=linux&arch=x64" -o /tmp/discord.deb \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends /tmp/discord.deb \
    && rm -rf /var/lib/apt/lists/* /tmp/discord.deb

WORKDIR /home/discord
VOLUME /home/discord
USER discord
CMD ["/usr/bin/dbus-run-session", "--", "/usr/bin/discord", "--no-sandbox", "--disable-setuid-sandbox", "--ozone-platform=wayland"]
