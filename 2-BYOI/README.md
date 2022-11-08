# Build Your Own Image

## Intro

傳統我們建置一台 VM 時會撰寫 shell scripts，來確保每台
VM 都能以同樣步驟進行安裝與設定。而 `Dockerfile` 定位就
非常類似於 shell scripts，讓開發者透過一份程式碼讓建置
容器映像檔 (Container Image) 的流程可重覆執行且自動化。

```dockerfile
FROM ubuntu:20.04

RUN apt update && apt install -y iputils-ping

ENTRYPOINT ["ping", "8.8.8.8"]
```

上面的範例就是利用 `ubuntu:20.04` 作為基底安裝 `iputils-ping`，最後當容器啟動時執行 `ping 8.8.8.8`。

## Build Image

Build image 時會將指定資料夾目錄的所有內容送到 Docker Daemon 內，
這可能導致浪費時間傳輸不重要的檔案。因此建置容器映像檔時會建立獨立資料夾放相關檔案。

```bash
demo/
├── Dockerfile
└── fake.cfg
```

* `demo/Dockerfile`
```dockerfile
FROM ubuntu:20.04

RUN apt update && apt install -y iputils-ping

RUN mkdir /etc/dummy

COPY fake.cfg /etc/dummy/fake.cfg

ENTRYPOINT ["ping", "8.8.8.8"]
```

以下指令是使用 `demo/Dockerfile` 作為藍本去建置名為 `test` 且 image tag 為 `0.1` 的 image，
而 `demo/` 底下的所有檔案內容會傳送到 Docker Daemon 以便 `COPY` 或 `ADD` 檔案進 image 內。

```bash
docker build -t test:0.1 -f demo/Dockerfile demo/
```

Build image 的過程:

```bash
Sending build context to Docker daemon   2.56kB
Step 1/5 : FROM ubuntu:20.04
 ---> ba6acccedd29
Step 2/5 : RUN apt update && apt install -y iputils-ping
 ---> Running in 83d7f6f1d06d
Get:1 http://security.ubuntu.com/ubuntu focal-security InRelease [114 kB]
Get:2 http://archive.ubuntu.com/ubuntu focal InRelease [265 kB]
...
Fetched 20.1 MB in 5s (4001 kB/s)
Reading package lists...
Building dependency tree...
....
Get:1 http://archive.ubuntu.com/ubuntu focal/main amd64 libcap2 amd64 1:2.32-1 [15.9 kB]
Get:2 http://archive.ubuntu.com/ubuntu focal/main amd64 libcap2-bin amd64 1:2.32-1 [26.2 kB]
Get:3 http://archive.ubuntu.com/ubuntu focal/main amd64 iputils-ping amd64 3:20190709-3 [40.1 kB]
Get:4 http://archive.ubuntu.com/ubuntu focal/main amd64 libpam-cap amd64 1:2.32-1 [8352 B]
debconf: delaying package configuration, since apt-utils is not installed
Fetched 90.5 kB in 1s (75.9 kB/s)
Selecting previously unselected package libcap2:amd64.
(Reading database ... 4127 files and directories currently installed.)
...
Setting up iputils-ping (3:20190709-3) ...
Processing triggers for libc-bin (2.31-0ubuntu9.2) ...
Removing intermediate container 83d7f6f1d06d
 ---> d2dee4d0d634
Step 3/5 : RUN mkdir /etc/dummy
 ---> Running in 2ce033ba16b0
Removing intermediate container 2ce033ba16b0
 ---> 7d619350a7fd
Step 4/5 : COPY fake.cfg /etc/dummy/fake.cfg
 ---> c95cb3d7568f
Step 5/5 : ENTRYPOINT ["ping", "8.8.8.8"]
 ---> Running in 75f274585ab9
Removing intermediate container 75f274585ab9
 ---> 8f197cd17876
Successfully built 8f197cd17876
Successfully tagged test:0.1
```

## DIY Time

請按照 [Install and Configure Nginx on Ubuntu](https://ubuntu.com/tutorials/install-and-configure-nginx#1-overview) 
步驟去撰寫 Dockerfile。