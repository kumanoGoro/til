# Docker とは
***コンテナ型***のアプリケーション実行環境
[Docker](https://www.docker.com/)

#### 今までと何が違うのか
コンテナ型以外には以下がある。
- ハイパーバイザ型（Hyper-Vなど）
- ホスト型（VMware PlayerやWindows Virtual PC、VirtualBoxなど）

 これらは仮想化されたハードウェア上で Linux OS が動作し、その上で目的のアプリケーションがプロセスとして動作する。アプリケーションを実行するためにはまずゲストOSを稼働させなければならず、起動に時間もかかるし、CPUやメモリ、ディスクなどのリソースも多く消費する。オーバーヘッドが大きく、リソースも無駄が多い。

→コンテナ型はホストOSの1プロセスとして動作するため、軽量・高速。

#### 実現するための技術
Linuxカーネルの以下の機能
- 名前空間の隔離機能
ファイルシステムやコンピュータ名、ユーザー名（ユーザーID）、グループ名（グループID）、プロセスID、ネットワーク機能などを、コンテナごとに独自に設定できるようにする機能。
- リソースの隔離機能
CPUやメモリ、ディスク入出力など、コンテナ内で利用するリソースを他のコンテナから隔離したり、設定に基づいて振り分けたりする機能。

 →Windows 上で Docker を使うには [Docker for Windows](https://docs.docker.com/docker-for-windows/) が必要。
 Windows 10（64bit版、Proエディション以上が必要）の仮想化環境「Hyper-V」上にLinuxコンテナ実行用の軽量OS（MobyLinux）をインストールして、その上でLinuxコンテナを実行する。

### 動かしてみる
[Docker for Windows のインストール](http://docs.docker.jp/windows/step_one.html) を参考にインストール。
Docker Quickstart Terminal を実行してみる。
```
Running pre-create checks...
(default) No default Boot2Docker ISO found locally, downloading the latest relea
se...
Error with pre-create check: "Get https://api.github.com/repos/boot2docker/boot2
docker/releases/latest: dial tcp: lookup api.github.com: getaddrinfow: This is u
sually a temporary error during hostname resolution and means that the local ser
ver did not receive a response from an authoritative server."
```

エラーが出た。通信に失敗している模様。プロキシの問題？

```
C:\Program Files\Docker Toolbox>set HTTP_PROXY=http://**********.co.jp:8
080

C:\Program Files\Docker Toolbox>set HTTPS_PROXY=http://**********.co.jp:
8080

C:\Program Files\Docker Toolbox>start.sh
```
```
Running pre-create checks...
(default) No default Boot2Docker ISO found locally, downloading the latest release...
(default) Latest release for github.com/boot2docker/boot2docker is v17.09.0-ce
(default) Downloading C:\Users\tie303229\.docker\machine\cache\boot2docker.iso from https://github.com/boot2docker/boot2docker/releases/download/v17.09.0-ce/boot2docker.iso...
(default) 0%.Error removing file: Error removing temporary download file: remove C:\Users\tie303229\.docker\machine\cache\boot2docker.iso.tmp697941079: The process cannot access the file because it is being used by another process.
(default)
Error with pre-create check: "read tcp 172.16.66.49:49513->172.26.64.1:8080: wsarecv: An existing connection was forcibly closed by the remote host."
Looks like something went wrong in step ´Checking if machine default exists´... Press any key to continue...
```

ダウンロードは進んだ。別のプロセスが掴んでいるからファイルが消せないとは。。。

```
λ docker version
Client:
 Version:      17.06.0-ce
 API version:  1.30
 Go version:   go1.8.3
 Git commit:   02c1d87
 Built:        Fri Jun 23 21:30:30 2017
 OS/Arch:      windows/amd64
error during connect: Get http://%2F%2F.%2Fpipe%2Fdocker_engine/v1.30/version: open //./pipe/docker_engine: The system cannot find the file specified. In the default daemon configuration on Windows, the docker client must be run elevated to connect. This error may also indicate that the docker daemon is not running.
```

エラーが出た。

[Cannot run docker from windows.](https://github.com/docker/toolbox/issues/636)で同じエラーの人がいる。


```
Command failed: C:\Program Files\Docker Toolbox\docker-machine.exe -D create -d virtualbox --virtualbox-memory 2048 default,Docker Machine Version: 0.12.2, build 9371605,Found binary path at C:\Program Files\Docker Toolbox\docker-machine.exe,Launching plugin server for driver virtualbox,Plugin server listening at address 127.0.0.1:57282,() Calling .GetVersion,Using API Version 1,() Calling .SetConfigRaw,() Calling .GetMachineName,(flag-lookup) Calling .GetMachineName,(flag-lookup) Calling .DriverName,(flag-lookup) Calling .GetCreateFlags,Found binary path at C:\Program Files\Docker Toolbox\docker-machine.exe,Launching plugin server for driver virtualbox,Plugin server listening at address 127.0.0.1:57286,() Calling .GetVersion,Using API Version 1,() Calling .SetConfigRaw,() Calling .GetMachineName,(default) Calling .GetMachineName,(default) Calling .DriverName,(default) Calling .GetCreateFlags,(default) Calling .SetConfigFromFlags,(default) Calling .PreCreateCheck,(default) DBG | COMMAND: C:\Program Files\Oracle\VirtualBox\VBoxManage.exe --version,(default) DBG | STDOUT:,(default) DBG | {,(default) DBG | 5.1.24r117012,(default) DBG | },(default) DBG | STDERR:,(default) DBG | {,(default) DBG | },(default) DBG | Hyper-V is not installed.,(default) DBG | %!(EXTRA *exec.Error=exec: "vmms.exe": executable file not found in %PATH%)COMMAND: wmic cpu get VirtualizationFirmwareEnabled,(default) DBG | Couldn't check that VT-X/AMD-v is enabled. Will check that the vm is properly created: exit status 2147749911,Error with pre-create check: "read tcp 172.16.66.49:57290->172.26.64.1:8080: wsarecv: An existing connection was forcibly closed by the remote host.",open C:\Users\tie303229\.docker\machine\machines\default\default\Logs\VBox.log: The system cannot find the path specified.,notifying bugsnag: [Error with pre-create check: "read tcp 172.16.66.49:57290->172.26.64.1:8080: wsarecv: An existing connection was forcibly closed by the remote host."],
```








- docker images
 docker imagesはローカルに保存されているDockerイメージの一覧を調べるためのコマンドである。

- docker inspect
docker inspectはイメージファイルの詳細情報を表示させるためのコマンドである。コンテナ起動時に何を実行するようになっているか、などが分かる。

- docker history
docker historyはイメージのレイヤー構成などを表示するコマンドである（レイヤーについては第1回の「Dockerイメージの履歴管理」を参照）。

- docker commit
現在のイメージファイルをベースにして、新しいイメージを作成するためのコマンドである。新イメージとして保存しておくと、カスタマイズや設定変更などを行ったコンテナを以後、簡単に起動できるようになる。

- docker build
　Dockerfileに基づいて、新しいイメージを作成するためのコマンドである。詳細は後述の「Dockerイメージを作成する」の節を参照のこと。

- docker rmi
　docker rmiは不要になったローカルのイメージを削除するコマンドである。


