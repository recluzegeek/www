---
title: Containerize Cursor
author: recluzegeek
pubDatetime: 2025-08-30T03:33:15Z
modDatetime: 2025-08-28T23:01:06.130Z
slug: containerize-cursor
featured: true
draft: false
tags:
  - containerization
  - linux
description: Containerize GUI Apps in Linux Containers
---

Lately, I've been into running GUI (Graphical User Interface) apps inside containers especially in Linux, and that too single GUI apps without full fledged Desktop Environments for fast boot and startup times.
My initial motivation was to run Cursor inside a container, bypassing the trial limit and too many trials on this machine error. Some folks may point me towards yx-elite/cursor-limit-reset @github,
but this workaround may work sometimes and not always guaranteed and is always messy. So, I look towards google started looking CURSOR in CLOUD, as VSCODE is in GITHUB CODESPACES or similar
product launched by GOOGLE PROJECT IDX, but my progress was halted as there was none. I starting digging on how github codespaces is being hosted in the cloud, and my initial instincts
were they may use virtualization, or some advanced K8s setup and other stuff that may not be available for masses + huge computing machinery available to them.

I changed my google query to `run gui apps in linux containers`, and to be honest, I would say I was not disappointed, folks already have been working on it way before the idea hit me, so
I had some hopes alive. Many folks online at stackoverflow and several discussions pointed towards this short blog [Running GUI apps with Docker by FABIOREHM](https://fabiorehm.com/blog/2014/09/11/running-gui-apps-with-docker/), and it
laid foundational work for me. I keep on digging and hit goldmine, where nerds have already made firefox containerized and this is where I hit goldmine again starting from my modified google query
`lightweight graphical linux container`:

1. [Running GUI Applications in Docker Containers by Priyam Sanodiya](https://medium.com/@priyamsanodiya340/running-gui-applications-in-docker-containers-a-step-by-step-guide-335b54472e4b)
1. [mviereck/x11docker](https://github.com/mviereck/x11docker/)
1. [jlesage/docker-baseimage-gui](https://github.com/jlesage/docker-baseimage-gui)

`mviereck/x11docker` is an excellent resource for someone looking to run Desktop Environments inside Linux containers. `jlesage/docker-baseimage-gui` did a fantastic job and provided community
with a base GUI image, on top of which any GUI app could be containerized, with additional features, such as VNC, web authentication, secure transport, init scripts, environment variables,
reverse proxy, web audio pass and many other amazing features. Customizing container according to my needs was the most significant task I underwent, my initial task list or the challenges
I faced included:

1. Downloading and installing the Cursor editor in the container.
1. Have the container user, sudo access (root user privilege for administration purposes, which is blocked by default in containers)
1. Integrate the bash or zsh in the editor, since the default user `app`, have default `/sbin/nologin` shell, which only allows login, and no other stuff, like it doesn't provide
   any interface (tty) to execute any commands further
1. How to authenticate myself in Cursor, since when we click the Login button, it opens a browser where we authenticate ourselves on normal machines :), and this container is designed only for single application
   and doesn't have any modern browser installed (yes, I know about terminal browsers, but that is not the point of discussions here).

```shell
# Get the AppImage from Official Website
CURSOR_DOWNLOAD_URL=https://downloads.cursor.com/production/823f58d4f60b795a6aefb9955933f3a2f0331d7b/linux/x64/Cursor-1.5.5-x86_64.AppImage
```

You may use either Docker or Podman, for this writeup I'll be using Podman. We start by writing our Dockerfile by setting our base image for the container and installing required dependencies.

```Dockerfile
FROM docker.io/jlesage/baseimage-gui:debian-11-v4

RUN apt-get update && apt-get install -y \
    libpci3 \
    libegl1 \
    libgl1 \
    libgles2 \
    libxcomposite1 \
    libxdamage1 \
    libxrandr2 \
    libasound2 \
    locales \
    xdg-utils \
    fuse \
    libfuse2 \
    libnss3 \
    libatk-bridge2.0-0 \
    libgtk-3-0 \
    libxss1 \
    libatk1.0-0 \
    libcups2 \
    libdrm2 \
    libxkbcommon0 \
    libdbus-glib-1-2 \
    libxcb-dri3-0 \
    && apt-get clean
```

In the next step, we will copy our AppImage from our host to the container. We can setup Dockerfile in two ways, either we include downloading logic inside Dockerfile which would take extra
time each time we build the image, and the other is to copy it from the host to container, and this is much faster operation and Docker caching will be helpful in this scenario. I decided
to opt for the 2nd option, so go ahead and download the Cursor AppImage and place it along with the Dockerfile directory (Docker Context). You may download
via `curl -L $(CURSOR_APPIMAGE_LINK) -o Cursor.AppImage`, and then update Dockerfile with:

```Dockerfile
COPY Cursor.AppImage /tmp/Cursor.AppImage

# Extract AppImage contents at build time to avoid FUSE
RUN chmod +x /tmp/Cursor.AppImage && \
    /tmp/Cursor.AppImage --appimage-extract && \
    mv squashfs-root /opt/cursor && \
    rm /tmp/Cursor.AppImage

# Set app name for GUI container
RUN set-cont-env APP_NAME "Cursor"

# Copy and set permissions on launch script
COPY startapp.sh /startapp.sh
RUN chmod +x /startapp.sh

EXPOSE 5800 5800

ENTRYPOINT ["/init"]
```

Create new bash script file named `startapp.sh`, with the following content to execute Cursor on container startup:

```bash
#!/bin/bash

# Run app
exec /opt/cursor/AppRun
```

Make sure to set the `startapp.sh` script executable with `chmod +x startapp.sh`.

At this point of time, you've working Cursor installation inside of container, and you can build your image and run it to test it with:

```bash
# building the image, note the extra space between mycursor:1 and the dot (.)
# this is important!!! as it tells podman that current directory is the build context
podman build -t mycursor:1 .

# list images locally
podman images

# after building, we can run with
podman run -it \                                                                                                                                      took 1m38s
  -e APP_USER_ID=$(id -u) \
  -e APP_GROUP_ID=$(id -g) \
  -p 5800:5800 \
  -p 5900:5900 \
  localhost/mycursor:1
```

And now, you can visit `http://localhost:5800` in your favorite browser and you've Cursor in your browser, but wait you've new error :( saying

```bash
Unable to write program user data.

A system error occurred...
Please make sure the following directories are available:

/config/xdg/config/Cursor
~/.cursor/extensions
/tmp/run/user/app
```

Don't worry, you can easily fix by modifying the `startapp.sh` script to include the following code block before app execution

```bash
export HOME=/config
export XDG_CONFIG_HOME=/config/xdg/config
export XDG_CACHE_HOME=/config/xdg/cache
```

Build the Dockerfile again, and run it following the previous instructions, and then visit `http://localhost:5800` in your browser and you've hopefully Cursor in your browser, also ignore the logs as well :).

[](1st_image)

We've successfully completed the installation of Cursor inside the container, and now is the time to move on to the authenticating ourselves. Normally, we would click the Login button,
and Cursor will open a browser window or tab and authenticate us. But, inside this container, where there is only single application running and no other stuff is allowed. The solution
is simple, just **hover over the Login button and right click on it and copy the Login URL**, wait you tried to paste the link to browser and it didn't pasted. If you carefully see the left side
of the window, you can see three vertical dots (if you previously click the `x` button), click on it and you've your copied link there. Again copy it, and paste it in your host machine
browser and authenticate yourself.

[](2nd_image)

Great job! You've so far, containerized Cursor and authenticated yourself which you previously thought difficult. Now, let's move onto the next step giving our normal user `app` the admin power with `sudo`, so
we can install the packages we needed, run some admin tasks via the editor terminal.

In the `Dockerfile`, add two new packages in the packages installation section:

```bash
RUN apt-get update && apt-get install -y \
    libpci3 \
    libegl1 \
    libgl1 \
    ...
    ...
    ...
    sudo \
    bash \
    && apt-get clean
```

Create new bash script named `50-enable-sudo.sh`, make sure to start the filename with `50-whatever.sh` and place the following content in it.

```bash
#!/bin/bash

set -e

# Get app username
APP_UID=${USER_ID:-1000}
APP_GID=${GROUP_ID:-1000}
APP_USER=$(getent passwd "$APP_UID" | cut -d: -f1)

if [ -z "$APP_USER" ]; then
    echo "User with UID $APP_UID not found."
    exit 1
fi

# Add app user to sudo group and change shell to bash
#usermod -aG sudo "$APP_USER"

# Recreate sudo group if it’s missing
if ! getent group sudo > /dev/null; then
    echo "[sudo-setup] Creating missing 'sudo' group..."
    groupadd sudo
fi

# Add user to sudo group (optional if using sudoers file directly)
usermod -aG sudo app
#chsh -s /bin/bash "$APP_USER"

# Optionally, allow passwordless sudo
echo "$APP_USER ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/$APP_USER
chmod 0440 /etc/sudoers.d/$APP_USER
```

Modify the `Dockerfile` to include the `50-enable-sudo.sh` inside container with following snippet:

```Dockerfile
COPY 50-enable-sudo.sh /etc/cont-init.d/50-enable-sudo.sh
RUN chmod +x /etc/cont-init.d/50-enable-sudo.sh
```

Build and run the container to ensure there's no error.

[](3rd sudo image)

Up until this point, we have containerized cursor, knows how to authenticate ourselves, and give ourselves admin privileges with sudo.

The last puzzle is on how to overcome the limitation of `/sbin/nologin` default shell assigned to us. I can see two paths for us from here, that are:

1. Use the `sudo` power and change our default shell to `/bin/bash`
1. Change the default shell in CURSOR

For security and ease of doing, I would prefer 2nd option. Open the VSCode Settings inside Cursor Editor (ohh! the irony, VSCode Setting inside Cursor :), and search `linux` in the search bar
and you can see 3rd search result being `Terminal > Integerated > Default Profile > Linux`, change it from `null` to `bash`. Woooh! You've bash inside, the Cursor with `sudo` power,
and now you can start building amazing things.

## Tips

1. Use dedicated apps for remote accessing like `remmina`, instead of browser interface. You'll thank me later.
1. Use volume or bind mounts for easy file/folder sharing inside the container

## Cons

1. Since the Cursor runs inside a container, it may crashes sometimes due to resource limitations
