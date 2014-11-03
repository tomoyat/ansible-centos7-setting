# centos7の設定をする

## rootでやる設定

前提 : rootのpasswordでsshできる

- userの作成
- sudoの設定
- sshの設定
- sshdの再起動

終わったらrebootをかける必要がある

実行コマンド

```
ansible-playbook -i hosts root-site.yml -u root --ask-pass -c paramiko
```

###設定ファイル

- group_vars/all : ssh portの設定
- roles/user_add/vars/main.yml : userの設定
- hosts : hostの設定


###userの設定ファイル

[hashed password](http://docs.ansible.com/faq.html#how-do-i-generate-crypted-passwords-for-the-user-module)

```
# create home directory for new new user
create_homedir: true

users:
  - username: userName
    groups:
      - "wheel"
    password: "hashed pass word"
    ssh_key: "ssh public key"
```

## 作成したuserでやる設定

- iptablesの設定

実行コマンド

作成したユーザーとlocalのユーザ名が同じ場合

```
$ cat hosts
[servers]
xx.xx.xx.xx:sshd_port
$ ansible-playbook -i hosts site.yml
```
