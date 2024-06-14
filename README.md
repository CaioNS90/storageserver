# storageserver
#Criando volume 
[cloudshell-user@ip-10-130-55-145 ~]$ aws s3 ls
2024-04-30 13:46:09 aws-cloudtrail-logs-058264162874-7c1e0ebb
2024-01-31 12:41:57 luxxy-covid-testing-system-pdf-pt-lvvqurnp
[cloudshell-user@ip-10-130-55-145 ~]$ aws ec2 create-volume \
>     --volume-type gp3 \
>     --size 80 \
>     --availability-zone us-west-2b
{
    "AvailabilityZone": "us-west-2b",
    "CreateTime": "2024-06-14T21:48:24+00:00",
    "Encrypted": false,
    "Size": 80,
    "SnapshotId": "",
    "State": "creating",
    "VolumeId": "vol-0158f6e3320f2a19c",
    "Iops": 3000,
    "Tags": [],
    "VolumeType": "gp3",
    "MultiAttachEnabled": false,
    "Throughput": 125
}

#Attach volume
[cloudshell-user@ip-10-130-55-145 ~]$ aws ec2 attach-volume --volume-id vol-0158f6e3320f2a19c --instance-id i-0ef2c7cf4a68ac2c6 --device /dev/sdf
{
    "AttachTime": "2024-06-14T21:51:29.353000+00:00",
    "Device": "/dev/sdf",
    "InstanceId": "i-0ef2c7cf4a68ac2c6",
    "State": "attaching",
    "VolumeId": "vol-0158f6e3320f2a19c"
}

=====================================================================================================================================================================
# Montando disco no linux 

Primeiro é preciso saber quais são os discos que estão atachados nas maquinas
```bash
fdisk -l
```
Ex.

CRIANDO O PARTICIONAMENTO
Após identificar o disco, agora vamos criar a partição dentro do disco com o comando

```bash
fdisk <disco>
```
Ex. fdisk /dev/nvme2n1
Os comandos do FDISK são:
m - help
n - para a criação da partição
1 - Digita “p” para a criação da partição primária
2 - Pergunta o número da partição que vai de 1 - 4, por padrão é 1
3 - Pergunta pelo primeiro setor, por padrão é 2048.
4 - Por ultimo qual é o ultimo setor (pode digitar +size{K,M G} o tamanho que deseja, ou se preferir o disco inteiro aperta enter que ele define o padrão).
Ao final para gravar digita “w”
p - para listar as partições
q - para sair

FORMATANDO O DISCO
O comando para a formatação do disco é o:

```bash
mkfs.ext4 <partição criada>
```
ex.: mkfs.ext4 /dev/nvme0n1p1

MONTANDO A PARTIÇÃO
Precisa ter um diretório de destino
Criar um diretório para o ponto de montagem

```bash
mkdir <diretorio>
```
ex.: mkdir /montagem/
Para montar o disco digite o comando

```bash
mount -t <tipo> <partição> <diretorio de montagem>
```
ex.: mount -t ext4 /dev/nvme0n1p1 /montagem/
