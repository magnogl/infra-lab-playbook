# Configuração de chaves SSH para autenticação no GitHub

## 1. Configurar identidade do Git

```bash
git config --global user.name "seu_usuario"
git config --global user.email "seu@email.com"
```
Confirma com:
```bash
git config --list
```

## 2. Gerar o par de chaves SSH
```bash
ssh-keygen -t ed25519 -C "seu@email.com"
```

Quando perguntar onde salvar, apertar Enter para aceitar o padrão `~/.ssh/id_ed25519`

Depois define uma passphrase para proteger a chave.

## 3. Exibir a chave pública
```bash
cat ~/.ssh/id_ed25519.pub
```
Copia todo o conteúdo que aparecer.

## 4. Cadastrar a chave no GitHub
Acessa github.com/settings/keys, clica em New SSH Key, define um título identificando a máquina, cola a chave pública no campo Key e salva.

## 5. Testar a conexão
```bash
ssh -T git@github.com
```
Na primeira conexão vai perguntar se confia no host, digita yes. O retorno esperado é:
```bash
"Hi seu_usuario! You've successfully authenticated..."
```

