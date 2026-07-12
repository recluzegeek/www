+++
title = "GPU passthrough VM - Arch Linux"

date = 2026-07-10
updated = 2026-09-10
draft = true

[taxonomies]
tags = ["qemu", "tag2"]

[extra]
dek = "Passthrough GPU in QEMU/KVM VM - for better performance in VM, specially for sites like Figma, Meet, and Teams"
+++

## Motivation

I worked in VM on my host arch-machine for development work, however during online meetings on google-meet or teams - my CPU spikes over ~90% lagging considerble on firefox (to the point, i can't hear, any action take ~10secs to respond, audio/video doesn't transcribe properly), and as soon as i close the tab, lagging goes away - same pattern is noticed on figma site, it doesn't slows system most of the time, but it lags considerble, making me cry during online demo, so I decided to passthrough my machine GPU with my development vm machine, for better experience.

- installed on host

```bash
sudo pacman -S qemu-desktop spice-vdagent virglrenderer
```

- Open the VM settings
  - go to display section
    - change the listen type from address to none
    - check the opengl box
    - ensure the device dropdown selects your gpu
  - go to video
    - change the model dropdown to virtio
    - check the 3D acceleratoin button

- Install following inside Guest VM

```bash
sudo pacman -S mesa spice-vdagent xf86-video-vesa
```

- Do a reboot and everything should be working. My current setup:
  - arch + i3wm as host
  - arch + cinnamon as guest vm
- things may break under different specs
