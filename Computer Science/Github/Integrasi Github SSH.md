```shell
ssh-keygen -t ed25519 -C "your_email@example.com"
```

jika sudah ada file `config` cukup open file tsbt.

```shell
open ~/.ssh/config
```

jika tidak ada file `config` buat terlebih dahulu.

```shell
touch ~/.ssh/config
```

Tambahkan ssh key setup didalam file `config` tersebut.

```text
Host github.com
  AddKeysToAgent yes
  IdentityFile ~/.ssh/id_ed25519
```


copy ssh-key dari file yang telah dibuat tadi dengan:

```shell
pbcopy <nama file generate ssh>.pub
```
