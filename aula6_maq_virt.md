Máquinas Virtuais

Agenda

• O que são máquinas virtuais?

• Por que utilizar virtualização?

• Abordagens

• Virtualização de Processos

• Virtualização de Sistemas

• Ferramentas

Uma máquina virtual ou virtual machine (VM) é uma
duplicação eﬁciente e isolada de uma máquina

Conceito

• Duplicação : VM devem se comportar da mesma

forma que uma máquina real

• Programas não devem fazer distinção

• Isolação : Diversas VMs executam

independentemente

• Eﬁciência : VMs devem executar em velocidade

próxima do hardware real

VMs modelam uma máquina real com ﬁdelidade e eﬁciência, porém
precisam rodar ﬁsicamente na máquina que estão virtualizando

Tipos de VMs

• VMs de Processos

• VMs Nativas (Tipo 1)

• VMs de Sistemas (Tipo 2)

VMs de Processo

• Executam binários de aplicações com um conjunto
de instruções (ISA, Instruction Set Architecture)
diferente do hardware

Exemplo

• Java Virtual Machine

VMs Nativas

• Necessitam do hipervisor ou monitor de máquina

virtual para implementar a VM

• Hypervisor é uma camada de software entre o

hardware e o sistema operacional

• Oferece ao SO visitante (guest) a abstração da

máquina virtual

• Controla o acesso do guest aos recursos de

hardware

VMs Nativas

VMs de Sistema (Tipo 2)

• Fornecem um ambiente de SO completo

• Fundamentam-se em nível de instrução

• Diversos SOs podem co-existir em um único

hardware

Exemplo

Ferramentas

• Algumas das ferramentas de virtualização mais

comuns:

• VMWare vSphere Hypervisor

• VirtualBox

vSphere Hypervisor

• Executado como se fosse um programa no espaço
de aplicação, dentro de um SO hospedeiro (host)

• Responsável pela abstração do hardware
disponibilizado ao SO visitante (guest)

• Vídeo

VirtualBox

• VM Tipo 2

• Permite o uso de um SO como uma aplicação

• Utiliza de Disk Image Files para armazenamento

Aplicações

• Consolidação de servidores

• Laboratórios de Ensino

• Desenvolvimento de software

