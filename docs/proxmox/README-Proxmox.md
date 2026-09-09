# Instalação e Configuração do Proxmox VE

## Hardware utilizado

| Componente | Especificação |
|------------|---------------|
| Processador | AMD Ryzen 7 7200G |
| Memória RAM | 48GB |
| Armazenamento | 1x SSD 256GB + 2x SSD 480GB |

## Plano de armazenamento

- SSD 256GB: sistema Proxmox
- 2x SSD 480GB: ZFS Mirror para armazenamento de VMs

## Download da ISO

A ISO oficial do Proxmox VE pode ser baixada em:
[https://www.proxmox.com/en/downloads](https://www.proxmox.com/en/downloads)

## Gravação da ISO no pendrive

Identifica o pendrive com:

```bash
lsblk
```

Desmonta o pendrive:

```bash
sudo umount /dev/sdX
```

Grava a ISO:

```bash
sudo dd if=/caminho/da/imagem.iso of=/dev/sdX bs=4M status=progress
```

Aguarda finalizar e sincroniza o buffer:

```bash
sudo sync
```

## Configurações de rede definidas na instalação

| Campo | Valor |
|-------|-------|
| Hostname | pve.local |
| IP | 192.168.X.X/24 |
| Gateway | 192.168.X.1 |
| DNS | 1.1.1.1 |

## Validação pós instalação

Verifica o IP da interface de rede:

```bash
ip a
```

Testa conectividade com a internet:

```bash
ping 8.8.8.8
```

## Acesso à interface web

Acessa pelo navegador em qualquer máquina da rede:
```bash
https://192.168.x.x:8006
```
---
# No valid subscription
Essa mensagem de aviso é o comportamento padrão do Proxmox VE quando o servidor é inicializado sem uma chave de subscrição paga ativa

*Por que essa mensagem aparece?*
Por padrão, o Proxmox VE vem configurado de fábrica apontando para o **Proxmox VE Enterprise Repository** e para obter atualizações de segurança e novos recursos no seu laboratório sem receber erros do gerenciador de pacotes (APT), você precisa desativar o repositório Enterprise e habilitar o repositório No-Subscription

## Método 1: Pela Interface Web (Mais fácil)
No *menu esquerdo*, clique no seu nó do Proxmox.

Vá em *Updates* (Atualizações) *Repositories* (Repositórios).

Na lista inferior, selecione a linha que aponta para o repositório *pve-enterprise* e clique em *Disable* (Desativar).

Clique no botão *Add* (Adicionar) no topo da tabela, selecione *No-Subscription* no menu de opções e adicione.

(Opcional) Se você planeja utilizar o Ceph, desative também o repositório enterprise do Ceph e adicione a versão "Ceph No-Subscription".

## Método 2: Pela Linha de Comando (CLI)
Como você está instalando a versão mais recente (Proxmox VE 9, baseado no Debian 13 Trixie), o sistema utiliza o formato de repositórios modernos deb822 em arquivos .sources.

Abra o terminal do seu Proxmox (via SSH ou console de gerenciamento) e faça os seguintes ajustes:

Desative o Repositório Enterprise: Edite o arquivo /etc/apt/sources.list.d/pve-enterprise.sources e adicione a linha Enabled: no ao bloco do repositório, ou simplesmente comente o seu conteúdo.

Habilite o Repositório Sem Subscrição: Edite o arquivo /etc/apt/sources.list.d/proxmox.sources e configure o bloco para habilitar o repositório convencional:

    Types: deb
    URIs: http://download.proxmox.com/debian/pve
    Suites: trixie
    Components: pve-no-subscription
    Signed-By: /usr/share/keyrings/proxmox-archive-keyring.gpg

Atualize o Servidor: Por fim, execute a atualização do banco de dados de pacotes e aplique as correções do sistema:

    apt update
    apt full-upgrade -y

*Nota: Embora existam scripts comunitários populares na internet para remover permanentemente o pop-up de aviso do navegador alterando arquivos JavaScript internos do Proxmox (proxmoxlib.js), a documentação oficial do Proxmox não fornece ou apoia um procedimento nativo para remover este alerta visual.*

---

# Como remover a mensagem "Invalid Subscription" toda vez que loga na interface web

Essa mensagem aparece porque o Proxmox é gratuito mas tem um modelo de assinatur  enterprise. Sem licença paga ele mostra esse popup toda vez que loga na interface web.

A correção é simples, remove a verificação do arquivo JavaScript responsável pelo popup:
```bash
sed -i.bak "s/if (data.status !== 'Active')/if (false)/g" /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js
```
**O que é sed:**
Stream editor. Ferramenta que faz busca e substituição em arquivos de texto. Aqui estamos substituindo a condição que exibe o popup por false, fazendo ela nunca ser verdadeira.

```bash
-i.bak
```
Edita o arquivo original e cria um backup com extensão .bak antes de modificar. Se algo der errado, você tem o original.

Depois reinicia o serviço da interface web:
```bash
systemctl restart pveproxy
```
