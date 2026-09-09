
# Criação de VM Debian no Proxmox VE

## Especificações da VM

| Campo | Valor |
|-------|-------|
| Nome | debian-study |
| VM ID | 100 |
| ISO | debian-13.6.0-amd64-netinst.iso |
| Disco | 60GB (datastore ZFS Mirror) |
| CPU | 1 socket, 2 cores, tipo host |
| RAM | 4096 MiB (4GB) |
| Rede | vmbr0, VirtIO |

## Aba General

- **Node:** pve
- **VM ID:** 100
- **Name:** debian-study

## Aba OS

- **Storage:** local
- **ISO image:** debian-13.6.0-amd64-netinst.iso
- **Type:** Linux
- **Version:** 6.x - 2.6 Kernel

## Aba System

- **Machine:** q35
- **BIOS:** OVMF (UEFI)
- **EFI Storage:** datastore
- **Qemu Agent:** habilitado

## Aba Disks

- **Bus/Device:** SCSI 0
- **Storage:** datastore
- **Disk size:** 60 GiB
- **Cache:** Write back
- **Discard:** habilitado
- **IO thread:** habilitado

## Aba CPU

- **Sockets:** 1
- **Cores:** 2
- **Type:** host

## Aba Memory

- **Memory:** 4096 MiB

## Aba Network

- **Bridge:** vmbr0
- **Model:** VirtIO (paravirtualized)
- **Firewall:** habilitado

## Observações

> O tipo de CPU `host` expõe as instruções reais do processador para a VM, resultando em melhor performance para uso em lab.

> O storage `datastore` é o ZFS Mirror composto por 2x SSDs de 480GB configurado durante a instalação do Proxmox.
