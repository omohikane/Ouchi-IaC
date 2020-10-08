# Ouchi-IaC

自宅でIaCを実践してみたい､ということで立ててみたプロジェクト  
コンテンツを充実させていきたい yamahaルータ､おまえだぞ･･････  
Ansibleのベストプラクティスにそうと`main.yml`だらけになってわからなくなるため､ある程度ディレクトリ構造は踏襲しつつ変更している｡  
目的はお家IaCである程度管理しやすくすること｡

## Features  

お家でIaCをやる上で､使いたい物､作りたい物を実現していきます｡  
メインはAnsibleで管理して､なるべく手作業を減らすように努力したい｡  
ただし認証情報など隠しておきたい物も多々あるので､forkして使えるように考慮した構成にすることとする｡  
Debian系ディストリビューションばかりなのでコマンドはDebian系｡  

## Requirement  

主にLinuxサーバとAnsibleで構成しようと検討中  

- Linux(Debian family)  
- Ansible  

## Index  

目次的な物


## Usage  

SSHパスワード､Sudoパスワードを入れるのが面倒な場合､`group_vars/vault.yml`をAnsible Vaultで暗号化
Playbook実行時､暗号化した認証情報を利用するため`--extra-cars="@vault.yml"` をつけて暗号化したファイルを読み取るようにする｡ 
ansible.cfg の `ask_vault_pass = True` コメントアウトを外す｡  

```shell

$ ansible-playbook hogehoge.yml -i inventories/inventory  --extra-vars="@password.yml"
Vault password:

```

## Note  

- Ansibleの実行ユーザは?  
  group_var/all.yml 内の`ansible_ssh_user`で設定できる｡  
  操作対象に`{{ hoge }}`ユーザを作成し管理者権限を付与しておくと良い｡  

- SSHパスワードは?  
  group_var/vault.yml 内の`ansible_ssh_pass`で設定できる｡  
  鍵認証を推奨するが･･････  

- Sudoパスは?  
  group_var/vault.yml 内の`ansible_sudo_pass`で設定できる｡  
  暗号化を忘れないように
  
  


## Author  

* omohikane  
* twitter(@sattan72)  

## License  

MIT
