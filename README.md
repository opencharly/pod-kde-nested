# pod-kde-nested

A full **KDE Plasma** session nested inside another Wayland compositor, as one reusable
service. Compose the `kde-nested` candy into a pod that already has a compositor or
streamer, and Plasma appears in it:

```
<host compositor>  (headless sway, pixelflux, the cstream streamer, …)
  └─ kwin_wayland --wayland-display <host> --socket wayland-kde
        └─ plasmashell + KDE applications
```

```yaml
my-desktop:
    candy:
        require:
            - '@github.com/opencharly/pod-kde-nested:<tag>'
        env:
            KDE_NESTED_HOST_DISPLAY: wayland-1     # the host's socket
            KDE_NESTED_RUNTIME_DIR: /tmp/cstream-rt
```

| variable | default | meaning |
|---|---|---|
| `KDE_NESTED_HOST_DISPLAY` | `wayland-0` | the host compositor's Wayland socket |
| `KDE_NESTED_SOCKET` | `wayland-kde` | the socket KWin serves to Plasma and its apps |
| `KDE_NESTED_RUNTIME_DIR` | inherited `XDG_RUNTIME_DIR`, else `/tmp` | the `XDG_RUNTIME_DIR` holding both sockets |
| `KDE_NESTED_RESOLUTION` | empty | `WxH` of KWin's window (empty: the host decides) |
| `KDE_NESTED_BUS` | `session` | `session` (the pod's bus) or `private` |

Used by [pod-kde-vnc](https://github.com/opencharly/pod-kde-vnc),
[pod-kde-selkies](https://github.com/opencharly/pod-kde-selkies) and
[pod-kde-cstream](https://github.com/opencharly/pod-kde-cstream).
