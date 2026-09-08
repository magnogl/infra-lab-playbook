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

