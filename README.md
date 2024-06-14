# storageserver <br>
#Criando volume  <br>
[cloudshell-user@ip-10-130-55-145 ~]$ aws s3 ls <br>
2024-04-30 13:46:09 aws-cloudtrail-logs-058264162874-7c1e0ebb <br>
2024-01-31 12:41:57 luxxy-covid-testing-system-pdf-pt-lvvqurnp <br>
[cloudshell-user@ip-10-130-55-145 ~]$ aws ec2 create-volume \ <br>
>     --volume-type gp3 \
>     --size 80 \
>     --availability-zone us-west-2b
SAÍDA <br>
{
    "AvailabilityZone": "us-west-2b", <br>
    "CreateTime": "2024-06-14T21:48:24+00:00", <br>
    "Encrypted": false, <br>
    "Size": 80, <br>
    "SnapshotId": "", <br>
    "State": "creating", <br>
    "VolumeId": "vol-0158f6e3320f2a19c", <br>
    "Iops": 3000, <br>
    "Tags": [], <br>
    "VolumeType": "gp3", <br>
    "MultiAttachEnabled": false, <br>
    "Throughput": 125 <br>
}

#Attach volume <br>
[cloudshell-user@ip-10-130-55-145 ~]$ <br>
aws ec2 attach-volume <br>
--volume-id vol-0158f6e3320f2a19c <br>
--instance-id i-0ef2c7cf4a68ac2c6 <br>
--device /dev/sdf <br>

SAÍDA <br>
{
    "AttachTime": "2024-06-14T21:51:29.353000+00:00", <br>
    "Device": "/dev/sdf",
    "InstanceId": "i-0ef2c7cf4a68ac2c6", <br>
    "State": "attaching", <br>
    "VolumeId": "vol-0158f6e3320f2a19c" <br>
}

===================================================================================================================================================================== <br>
# Montando disco no linux <br>

Primeiro é preciso saber quais são os discos que estão atachados nas maquinas <br>
```bash
fdisk -l
```
Ex. <br>

CRIANDO O PARTICIONAMENTO <br>
Após identificar o disco, agora vamos criar a partição dentro do disco com o comando <br>

```bash
fdisk <disco>
```
Ex. fdisk /dev/nvme2n1 <br>
Os comandos do FDISK são: <br>
m - help <br>
n - para a criação da partição <br>
1 - Digita “p” para a criação da partição primária <br>
2 - Pergunta o número da partição que vai de 1 - 4, por padrão é 1 <br>
3 - Pergunta pelo primeiro setor, por padrão é 2048. <br>
4 - Por ultimo qual é o ultimo setor (pode digitar +size{K,M G} o tamanho que deseja, ou se preferir o disco inteiro aperta enter que ele define o padrão). <br>
Ao final para gravar digita “w” <br>
p - para listar as partições <br>
q - para sair

FORMATANDO O DISCO <br>
O comando para a formatação do disco é o: <br>

```bash
mkfs.ext4 <partição criada>
```
ex.: mkfs.ext4 /dev/nvme0n1p1 <br>

MONTANDO A PARTIÇÃO <br>
Precisa ter um diretório de destino <br>
Criar um diretório para o ponto de montagem <br>

```bash
mkdir <diretorio>
```
ex.: mkdir /montagem/ <br>
Para montar o disco digite o comando <br>

```bash
mount -t <tipo> <partição> <diretorio de montagem>
```
ex.: mount -t ext4 /dev/nvme0n1p1 /montagem/
