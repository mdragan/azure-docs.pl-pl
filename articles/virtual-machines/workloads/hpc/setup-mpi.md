---
title: Konfigurowanie interfejsu przekazywania komunikatów dla HPC — w usłudze Azure Virtual Machines | Dokumentacja firmy Microsoft
description: Dowiedz się, jak skonfigurować MPI for HPC na platformie Azure.
services: virtual-machines
documentationcenter: ''
author: vermagit
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 05/15/2019
ms.author: amverma
ms.openlocfilehash: 1717c0d95c00e13ae0d87ab920bc88cd4f0df978
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66809845"
---
# <a name="set-up-message-passing-interface-for-hpc"></a>Konfigurowanie interfejsu przekazywania komunikatów dla HPC

Komunikat przekazywania Interface (MPI) obciążenia to znaczną część tradycyjnych obciążeń obliczeń HPC. Funkcja SR-IOV włączone rozmiarów maszyn wirtualnych na platformie Azure Zezwalaj na prawie każdym flavor MPI, które ma być używany. 

Uruchamianie zadań MPI na maszynach wirtualnych wymaga skonfigurowania partycję kluczy (p-keys) w dzierżawie. Postępuj zgodnie z instrukcjami w [odnajdywanie klucze partycji](#discover-partition-keys) sekcji, aby uzyskać szczegółowe informacje na temat określania wartości p-key.

## <a name="ucx"></a>UCX

[UCX](https://github.com/openucx/ucx) oferuje najwyższą wydajność na IB i współdziałanie z usługami MPICH i OpenMPI.

```bash
wget https://github.com/openucx/ucx/releases/download/v1.4.0/ucx-1.4.0.tar.gz
tar -xvf ucx-1.4.0.tar.gz
cd ucx-1.4.0
./configure --prefix=<ucx-install-path>
make -j 8 && make install
```

## <a name="openmpi"></a>OpenMPI

Zainstaluj UCX, jak opisano wcześniej.

```bash
sudo yum install –y openmpi
```

Build OpenMPI.

```bash
wget https://download.open-mpi.org/release/open-mpi/v4.0/openmpi-4.0.0.tar.gz
tar -xvf openmpi-4.0.0.tar.gz
cd openmpi-4.0.0
./configure --with-ucx=<ucx-install-path> --prefix=<ompi-install-path>
make -j 8 && make install
```

Run OpenMPI.

```bash
<ompi-install-path>/bin/mpirun -np 2 --map-by node --hostfile ~/hostfile -mca pml ucx --mca btl ^vader,tcp,openib -x UCX_NET_DEVICES=mlx5_0:1  -x UCX_IB_PKEY=0x0003  ./osu_latency
```

Klucz partycji należy sprawdzić, jak wspomniano powyżej.

## <a name="mpich"></a>MPICH

Zainstaluj UCX, jak opisano wcześniej.

Build MPICH.

```bash
wget http://www.mpich.org/static/downloads/3.3/mpich-3.3.tar.gz
tar -xvf mpich-3.3.tar.gz
cd mpich-3.3
./configure --with-ucx=<ucx-install-path> --prefix=<mpich-install-path> --with-device=ch4:ucx
make -j 8 && make install
```

Uruchamianie MPICH.

```bash
<mpich-install-path>/bin/mpiexec -n 2 -hostfile ~/hostfile -env UCX_IB_PKEY=0x0003 -bind-to hwthread ./osu_latency
```

Klucz partycji należy sprawdzić, jak wspomniano powyżej.

## <a name="mvapich2"></a>MVAPICH2

Build MVAPICH2.

```bash
wget http://mvapich.cse.ohio-state.edu/download/mvapich/mv2/mvapich2-2.3.tar.gz
tar -xv mvapich2-2.3.tar.gz
cd mvapich2-2.3
./configure --prefix=<mvapich2-install-path>
make -j 8 && make install
```

Uruchamianie MVAPICH2.

```bash
<mvapich2-install-path>/bin/mpirun_rsh -np 2 -hostfile ~/hostfile MV2_CPU_MAPPING=48 ./osu_latency
```

## <a name="platform-mpi-community-edition"></a>Platforma MPI Community Edition

Zainstaluj wymagane pakiety dla Platform MPI.

```bash
sudo yum install libstdc++.i686
sudo yum install glibc.i686
Download platform MPI at https://www.ibm.com/developerworks/downloads/im/mpi/index.html 
sudo ./platform_mpi-09.01.04.03r-ce.bin
```

Postępuj zgodnie z procesem instalacji.

## <a name="intel-mpi"></a>Intel MPI

[Pobierz Intel MPI](https://software.intel.com/mpi-library/choose-download).

Zmień ustawienia zmiennej środowiskowej I_MPI_FABRICS w zależności od wersji. Intel MPI 2018, można użyć `I_MPI_FABRICS=shm:ofa` w przypadku 2019 r, użyj `I_MPI_FABRICS=shm:ofi`.

Przypinanie proces działa prawidłowo dla 15, 30 i 60 Samodzielnym domyślnie.

## <a name="osu-mpi-benchmarks"></a>Testy porównawcze OSU MPI

[Pobierz testy porównawcze OSU MPI] [ http://mvapich.cse.ohio-state.edu/benchmarks/ ](http://mvapich.cse.ohio-state.edu/benchmarks/) i untar.

```bash
wget http://mvapich.cse.ohio-state.edu/download/mvapich/osu-micro-benchmarks-5.5.tar.gz
tar –xvf osu-micro-benchmarks-5.5.tar.gz
cd osu-micro-benchmarks-5.5
```

Tworzenie testów porównawczych, za pomocą określonej biblioteki MPI:

```bash
CC=<mpi-install-path/bin/mpicc>CXX=<mpi-install-path/bin/mpicxx> ./configure 
make
```

Testy porównawcze MPI podlegają `mpi/` folderu.


## <a name="discover-partition-keys"></a>Odkryj kluczy partycji

Odkryj klucze partycji (p-keys) do komunikacji z innymi maszynami wirtualnymi.

```bash
/sys/class/infiniband/mlx5_0/ports/1/pkeys/0
/sys/class/infiniband/mlx5_0/ports/1/pkeys/1
```

Większy z nich jest klucza dzierżawy, który powinien być używany z MPI. Przykład: Jeśli dostępne są następujące klucze p, 0x800b powinna służyć MPI.

```bash
cat /sys/class/infiniband/mlx5_0/ports/1/pkeys/0
0x800b
cat /sys/class/infiniband/mlx5_0/ports/1/pkeys/1
0x7fff
```

Użyj partycji innej niż domyślny (0x7fff) Klucz partycji. UCX wymaga BITEM p-key, zostaje wyczyszczona. Na przykład ustawić UCX_IB_PKEY jako 0x000b dla 0x800b.


## <a name="set-up-user-limits-for-mpi"></a>Konfigurowanie limitów użytkowników dla MPI

Konfigurowanie limitów użytkowników dla MPI.

```bash
cat << EOF >> /etc/security/limits.conf
*               hard    memlock         unlimited
*               soft    memlock         unlimited
*               hard    nofile          65535
*               soft    nofile          65535
EOF
```


## <a name="set-up-ssh-keys-for-mpi"></a>Konfigurowanie kluczy SSH dla MPI

Konfigurowanie kluczy SSH dla typów MPI, które tego wymagają.

```bash
ssh-keygen -f /home/$USER/.ssh/id_rsa -t rsa -N ''
cat << EOF > /home/$USER/.ssh/config
Host *
    StrictHostKeyChecking no
EOF
cat /home/$USER/.ssh/id_rsa.pub >> /home/$USER/.ssh/authorized_keys
chmod 644 /home/$USER/.ssh/config
```

Powyższej składni zakłada udostępnionego katalogu macierzystego, innego katalogu .ssh muszą zostać skopiowane do każdego węzła.

## <a name="next-steps"></a>Kolejne kroki

Dowiedz się więcej o [HPC](https://docs.microsoft.com/azure/architecture/topics/high-performance-computing/) na platformie Azure.
