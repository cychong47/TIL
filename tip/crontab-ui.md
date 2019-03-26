# crontab-ui

## get the docker image

```
cychong@mini1:~$ docker pull alseambusher/crontab-ui
Using default tag: latest
latest: Pulling from alseambusher/crontab-ui
019300c8a437: Pull complete
36e550278791: Pull complete
7b6010086a3a: Pull complete
b8af1efbc40d: Pull complete
ccec90dfb4aa: Pull complete
52ca2b3fd66c: Pull complete
Digest: sha256:f26eb7324abb2cab34f11f323b88ae523df8c4f017d6c803580bad42c6e431ca
Status: Downloaded newer image for alseambusher/crontab-ui:latest
```

## run a container

```
cychong@mini1:~$ sudo docker run -d -p 8000:8000 alseambusher/crontab-ui
ebfd3dde594971b3da1ffc8cedd1233990f39ea445edc2fd746ce9d964bdf2f8
```

## Reference

* https://github.com/alseambusher/crontab-ui