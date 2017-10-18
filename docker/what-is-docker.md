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
(default) No default Boot2Docker ISO found locally, downloading the latest release...
Error with pre-create check: "Get https://api.github.com/repos/boot2docker/boot2docker/releases/latest: dial tcp: lookup api.github.com: getaddrinfow: This is usually a temporary error during hostname resolution and means that the local server did not receive a response from an authoritative server."
```

エラーが出た。通信に失敗している模様。プロキシの問題？

```
C:\Program Files\Docker Toolbox>set HTTP_PROXY=http://**********.co.jp:8080
C:\Program Files\Docker Toolbox>set HTTPS_PROXY=http://**********.co.jp:8080
C:\Program Files\Docker Toolbox>start.sh
```

```
Running pre-create checks...
(default) No default Boot2Docker ISO found locally, downloading the latestrelease...
(default) Latest release for github.com/boot2docker/boot2docker is v17.09.0-ce
(default) Downloading C:\Users\tie303229\.docker\machine\cache\boot2docker.iso from https://github.com/boot2docker/boot2docker/releases/download/v17.09.0-ce/boot2docker.iso...
(default) 0%.Error removing file: Error removing temporary download file: remove C:\Users\tie303229\.docker\machine\cache\boot2docker.iso.tmp697941079: The process cannot access the file because it is being used by another process.
(default)
Error with pre-create check: "read tcp 172.16.66.49:49513->172.26.64.1:8080: wsarecv: An existing connection was forcibly closed by the remote host."
Looks like something went wrong in step ´Checking if machine default exists´... Press any key to continue...
```

ダウンロードは進んだ。別のプロセスが掴んでいるからファイルが消せないとは。。。
色々調べていたらアンチウィルスソフトが掴んでいるとか。

フォーラム見ていると
"I have solved the problem.
The boot2docker.iso should be located under c:\user\USERNAME.docker\machine\cache"
という声が。なるほどダウンロードして置いてみよう。
https://forums.docker.com/t/pre-create-check-failed-when-first-time-launch-docker-quickstart-terminal/9977/3

ダウンロードして再度実行。
```
Running pre-create checks...
Creating machine...
(default) Copying C:\Users\*****\.docker\machine\cache\boot2docker.iso to C:\Users\*****\.docker\machine\machines\default\boot2docker.iso...
(default) Creating VirtualBox VM...
(default) Creating SSH key...
(default) Starting the VM...
(default) Check network to re-create if needed...
(default) Windows might ask for the permission to create a network adapter. Sometimes, such confirmation window is minimized in the taskbar.
(default) Found a new host-only adapter: "VirtualBox Host-Only Ethernet Adapter #2"
(default) Windows might ask for the permission to configure a network adapter. Sometimes, such confirmation window is minimized in the taskbar.
(default) Windows might ask for the permission to configure a dhcp server. Sometimes, such confirmation window is minimized in the taskbar.
(default) Waiting for an IP...
Waiting for machine to be running, this may take a few minutes...
Detecting operating system of created instance...
Waiting for SSH to be available...
Detecting the provisioner...
Provisioning with boot2docker...
Copying certs to the local machine directory...
Copying certs to the remote machine...
Setting Docker configuration on the remote daemon...
Checking connection to Docker...
Docker is up and running!
To see how to connect your Docker Client to the Docker Engine running on this virtual machine, run: C:\Program Files\Docker Toolbox\docker-machine.exe env default



                        ##         .
                  ## ## ##        ==
               ## ## ## ## ##    ===
           /"""""""""""""""""\___/ ===
      ~~~ {~~ ~~~~ ~~~ ~~~~ ~~~ ~ /  ===- ~~~
           \______ o           __/
             \    \         __/
              \____\_______/

docker is configured to use the default machine with IP 192.168.99.100
For help getting started, check out the docs at https://docs.docker.com

Start interactive shell

*****@**********1 MSYS ~
$
```
クジラが出た！

