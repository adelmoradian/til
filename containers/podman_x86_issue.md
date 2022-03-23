# Podman M1 X86 issue

Docker can run on Apple M1 a container built on X86 but Podman can't

```bash
$ docker --version
Docker version 20.10.8, build 3967b7d

$ docker run -ti senseyeio/alpine-aws-cli echo ok
WARNING: The requested image's platform (linux/amd64) does not match the detected host platform (linux/arm64/v8) and no specific platform was requested
ok

$ podman --version
podman version 3.4.1

$ podman machine list 
NAME                     VM TYPE     CREATED         LAST UP            CPUS        MEMORY      DISK SIZE
podman-machine-default*  qemu        34 minutes ago  Currently running  1           2.147GB     10.74GB

$ podman run -ti senseyeio/alpine-aws-cli echo ok
{"msg":"exec container process `/bin/echo`: Exec format error","level":"error","time":"2021-10-31T01:54:47.000542236Z"
```

Solution is to install qemu-user-static on the Fedora CoreOS, this would just work, although would be slow.

```bash
$ podman machine ssh sudo rpm-ostree install qemu-user-static
$ podman machine ssh sudo systemctl reboot
$ podman-remote run -it arm64v8/alpine
/ # exit
```

