# Ouchi-IaC

自宅でIaCを実践してみたい､ということで立ててみたプロジェクト  
コンテンツを充実させていきたい yamahaルータ､おまえだぞ･･････  
目的はお家IaCである程度管理しやすくすること｡

## Features  

お家でIaCをやる上で､使いたい物､作りたい物を実現していきます｡  
メインはAnsibleで管理して､なるべく手作業を減らすように努力したい｡  
ただし認証情報など隠しておきたい物も多々あるので､各環境で使えるように考慮した構成にすることとする｡  
個人で使っているのはDebian系ディストリビューションばかりなのでコマンドはDebian系｡  

## Requirement  

主にLinuxサーバとAnsibleで構成しようと検討中  

- Linux(Debian family)  
  Ubuntu 20.04
  RaspberryPi OS
  
- Ansible  
  ansible                           3.1.0
  ansible-base                      2.10.7

## Index  

目次的な物

### Ansible

Ansible周りの設定｡  
できるだけ手動での変更は避けて運用する｡  

#### roles

各ロールの説明｡  
基本的にplaybookのみ置くようにする｡
複数のOS等またがっているので実行機器を限定するなど制御するようにする｡  

- common
  - user
    ユーザの追加､削除をする｡  

  - ssh
    sshd_conf の設定をする｡  

  - storage
    Storageの追加､マウントポイント作成とfstab編集｡  

  - package
    インストールするパッケージの設定｡  

- chinachu/epgstation/mirakurun
  録画システム｡  

- chrony
  NTPサーバ｡  
  家庭内の機器すべてが参照できるようにするのが理想｡  

- dnsmasq  
  内向きDNSサーバとして利用する｡  
  Dockerを使って環境依存しないようにする｡  

- docker
  dockerのインストール､セットアップなど｡  

- samba
  ファイル共有サーバ｡  

#### template

rolesで使う変数や材料などいろいろ入れる｡  
変更が多いので浅い階層に持ってきている｡  
rolesで使う場合は相対パスで記載するようにする｡  

#### group_vars

sshをするユーザやパスワードの指定など｡  

#### inventories

ansibleで管理する危機を記載する｡  

## Usage  

設定項目など使い方説明｡  

- Ansible

SSHパスワード､Sudoパスワードを入れるのが面倒な場合､`group_vars/vault.yml`をAnsible Vaultで暗号化  
Playbook実行時､暗号化した認証情報を利用するため`--extra-vars="@vault.yml"` をつけて暗号化したファイルを読み取るようにする｡  
ansible.cfg の `ask_vault_pass = True` コメントアウトを外す｡  

```shell

$ ansible-playbook hogehoge.yml -i inventories/inventory  --extra-vars="@password.yml"
Vault password:

```

## Note  

- Ansibleの実行ユーザは?  
  group_var/all.yml 内の`ansible_ssh_user`で設定できる｡  
  操作対象に`{{ hoge }}`ユーザを作成し管理者権限を付与しておくと良い｡  
  
  ```shell
  for ubuntu
  $ sudo adduser {{ hoge }}
  $ sudo gpasswd -a {{ hoge }} sudo
  $ sudo passwd {{ hoge }}
  ```

  ```shell
  for centos
  $ sudo useradd {{ hoge }}
  $ sudo gpasswd -a {{ hoge }} wheel
  $ sudo passwd {{ hoge }}
  ```

- SSHパスワードは?  
  group_var/vault.yml 内の`ansible_ssh_pass`で設定できる｡  
  鍵認証を推奨するが･･････  

- Sudoパスは?  
  group_var/vault.yml 内の`ansible_sudo_pass`で設定できる｡  
  暗号化を忘れないように

## Author  

- omohikane  

- twitter(@sattan72)  

## License  

MIT
