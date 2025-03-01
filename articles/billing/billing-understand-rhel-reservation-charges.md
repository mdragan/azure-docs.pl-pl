---
title: Zrozumienie plan rezerwacją firmy Red Hat i użycia — Azure | Dokumentacja firmy Microsoft
description: Dowiedz się, jak Red Hat plan rabaty są stosowane do oprogramowania Red Hat na maszynach wirtualnych.
services: billing
documentationcenter: ''
author: yashesvi
manager: yashar
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/22/2019
ms.author: cwatson
ms.openlocfilehash: fe0d0f0baa2b3d1c08e871541dce1511e00f7f87
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60370206"
---
# <a name="understand-how-the-red-hat-linux-enterprise-software-reservation-plan-discount-is-applied-for-azure"></a>Zrozumienie, jak Red Hat Linux Enterprise software plan rabatu związanego z rezerwacją jest stosowany dla platformy Azure

Po możesz kupić plan w systemie Red Hat Linux, rabat jest automatycznie stosowany do wdrożonych maszyn wirtualnych Red Hat (VM), które odpowiadają rezerwacji. Plan w systemie Red Hat Linux po awarii obejmuje koszt z uruchamiania oprogramowania firmy Red Hat na Maszynie wirtualnej platformy Azure.

Aby kupić właściwego planu w systemie Red Hat Linux, musisz zrozumieć, jakie Red Hat maszyny wirtualne można i liczba procesorów wirtualnych na tych maszynach wirtualnych. Następujące sekcje zawierają informacje ułatwiające identyfikację z pliku CSV użycia, co to jest plan zakupu.

## <a name="discount-applies-to-different-vm-sizes"></a>Rabat ma zastosowanie do różnych rozmiarów maszyn wirtualnych

Podobnie jak zarezerwowanych wystąpień maszyn wirtualnych Red Hat planu zakupów oferuje elastyczność rozmiaru wystąpienia. Oznacza to, że ma zastosowanie rabat, nawet gdy wdrożysz maszynę Wirtualną wraz z liczbą różnych procesorów wirtualnych. Rabat stosuje się do różnych rozmiarów maszyn wirtualnych w ramach planu oprogramowania.

Rabat zależy od współczynnik wymienione w poniższych tabelach. Współczynnik porównanie względnej śladu w ramach każdego miernika, w tej grupie. Współczynnik zależy od wirtualnych procesorów CPU maszyny Wirtualnej. Użyj wartości współczynnik, aby obliczyć liczbę wystąpień maszyn wirtualnych Uzyskaj rabat w wysokości planu w systemie Red Hat Linux.

Na przykład jeśli kupisz planu dla Red Hat Linux Enterprise Server dla maszyny Wirtualnej z 3 lub 4 Vcpu stosunek dla tej rezerwacji to 2. Rabat po awarii obejmuje koszt oprogramowania Red Hat dla:

- 2 wdrożonych maszyn wirtualnych z 1 lub 2 procesorów wirtualnych
- 1 wdrożonych maszyn wirtualnych z 3 lub 4 Vcpu,
- lub 0.77 lub wkrótce % 77 maszyny wirtualnej z 5 lub więcej procesorów wirtualnych.

Współczynnik 5 lub więcej procesorów wirtualnych Vcpu jest 2.6. Dlatego Rezerwacja Red Hat za pomocą maszyny Wirtualnej z 5 lub więcej procesorów wirtualnych Vcpu obejmuje tylko część koszt oprogramowania, czyli około 77%.

## <a name="understand-red-hat-vm-usage-before-you-buy"></a>Opis przed zakupem Red Hat w maszynie Wirtualnej

W poniższych tabelach przedstawiono plany oprogramowania, które można kupić rezerwację dla ich użycia skojarzone liczniki i współczynniki dla każdego.

### <a name="red-hat-enterprise-linux"></a>Red Hat Enterprise Linux

Nazwy witryny portalu Azure marketplace:

- Red Hat Enterprise Linux 6.7
- Red Hat Enterprise Linux 6.8
- Red Hat Enterprise Linux 6.9
- Red Hat Enterprise Linux 6.10
- Red Hat Enterprise Linux 7.2
- Red Hat Enterprise Linux 7.3
- Red Hat Enterprise Linux 7.4
- Red Hat Enterprise Linux 7.5
- Red Hat Enterprise Linux 7.6
- Red Hat Enterprise Linux 7 (lvm najnowsze)

|Red Hat maszyny Wirtualnej | MeterId| Współczynnik| Przykład rozmiar maszyny Wirtualnej|
| -------| ------------------------| --- |--- |
|1 – 4 vCPU licencji maszyny Wirtualnej|077a07bb-20f8-4bc6-b596-ab7211a1e247|1|D4s_v3|
|1 – 4 vCPU licencji maszyny Wirtualnej|2f96d035-3bac-46d6-b2bc-c6daa0938536|1|D4s_v3|
|1 – 4 vCPU licencji maszyny Wirtualnej|4831a7b4-bdd4-48a2-8e95-18d053971ede|1|D4s_v3|
|5 + procesor vCPU licencji maszyny Wirtualnej|291b2cbc-6c34-4e2b-a4e4-1ff8c106f672|2.166666667|D8s_v3|
|5 + procesor vCPU licencji maszyny Wirtualnej|3b6661c4-03dd-45e7-88c9-512fcb7906d5|2.166666667|D8s_v3|
|5 + procesor vCPU licencji maszyny Wirtualnej|037eddc0-fedd-4d73-b5d8-92fba9edb831|2.166666667|D8s_v3|
|5 + procesor vCPU licencji maszyny Wirtualnej|432cdeee-4034-4ddf-9ba4-9250a19b0d5f|2.166666667|D8s_v3|
|5 + procesor vCPU licencji maszyny Wirtualnej|794dcb90-0793-43e6-9909-70d29974e56d|2.166666667|D8s_v3|
|5 + procesor vCPU licencji maszyny Wirtualnej|86b5b0b4-3c19-4720-82e9-874f8c58b48e|2.166666667|D8s_v3|
|5 + procesor vCPU licencji maszyny Wirtualnej|86c35ec3-0a48-426a-9625-22d80e6ea55b|2.166666667|D8s_v3|
|5 + procesor vCPU licencji maszyny Wirtualnej|8b698c7a-47f1-4cba-8ae1-9853d5ad562d|2.166666667|D8s_v3|
|5 + procesor vCPU licencji maszyny Wirtualnej|a4daffb4-96f4-4fc5-b1e6-fd3a2cf3595e|2.166666667|D8s_v3|
|5 + procesor vCPU licencji maszyny Wirtualnej|a838cfb1-0bd3-4965-84f0-663f49afc2e2|2.166666667|D8s_v3|
|5 + procesor vCPU licencji maszyny Wirtualnej|99aed7b9-a0a9-4783-b90c-be7c2f3c7e30|2.166666667|D8s_v3|
|5 + procesor vCPU licencji maszyny Wirtualnej|d09f877e-03b4-48b2-b11a-782b965cff19|2.166666667|D8s_v3|
|Procesor wirtualny vCPU 44 licencji maszyny Wirtualnej|6f44ae85-a70e-44be-83ec-153a0bc23979|2.166666667||
|60 procesora wirtualnego vCPU licencji maszyny Wirtualnej|b9edcc5b-a429-4778-bc5a-82e7fa07fe55|2.166666667||

### <a name="red-hat-enterprise-linux-for-sap-with-ha"></a>Red Hat Enterprise Linux for SAP z wysokiej dostępności

Nazwa witryny portalu Azure marketplace:

|Red Hat maszyny Wirtualnej | MeterId | Współczynnik|Przykład rozmiar maszyny Wirtualnej|
| ------- | --- | ------------------------| --- | --- |
|1 – 4 vCPU licencji maszyny Wirtualnej |4d902611-eed7-4060-a33e-3c7fdbac6406|1|D4s_v3|
|5 + procesor vCPU licencji maszyny Wirtualnej|6dfb482b-23ea-487f-810c-e66360f025de|2.333333333|D8s_v3|

### <a name="red-hat-enterprise-linux-with-ha"></a>Red Hat Enterprise Linux z wysoką dostępnością

Nazwy witryny portalu Azure marketplace:

|Red Hat maszyny Wirtualnej | MeterId | Współczynnik|Przykład rozmiar maszyny Wirtualnej|
| ------- |------------------------| --- | --- |
|1 – 4 vCPU licencji maszyny Wirtualnej|e9711132-d9d9-450c-8203-25cfc4bce8de|1|D4s_v3|
|5 + procesor vCPU licencji maszyny Wirtualnej|93954aa4-b55f-4b7b-844d-a119d6bf3c4e|2|D8s_v3|

### <a name="rhel-for-sap-business-applications"></a>RHEL for SAP Business Applications

Nazwy witryny portalu Azure marketplace:

- Red Hat Enterprise Linux 6.8 for SAP Business Apps
- Red Hat Enterprise Linux 7.3 for SAP Business Apps
- Red Hat Enterprise Linux 7.4 for SAP
- Red Hat Enterprise Linux w wersji 7.5 dla rozwiązania SAP

|Red Hat maszyny Wirtualnej | MeterId | Współczynnik|Przykład rozmiar maszyny Wirtualnej|
| ------- |------------------------| --- |--- |
|1 Procesor wirtualny licencji maszyny Wirtualnej|25889e91-c740-42ac-bc52-6b8f73b98575|1|D2s_v3|
|2 procesory vCPU licencji maszyny Wirtualnej|2a0c92c8-23a7-4dc9-a39c-c4a73a85b5da|1|D2s_v3|
|4 vCPU licencji maszyny Wirtualnej|875898d3-3639-423c-82c1-38846281b7e8|1|D4s_v3|
|6 procesora wirtualnego vCPU licencji maszyny Wirtualnej|69a140fa-e08e-415c-85f2-48158e4c73a0|2.166666667||
|8 procesorów wirtualnych licencji maszyny Wirtualnej|777a5a74-22d6-48c9-9705-ac38fe05a278|2.166666667|D8s_v3|
|12 procesorów wirtualnych licencji maszyny Wirtualnej|d6b8917a-5127-497a-9f48-1e959df98812|2.166666667||
|16 vCPU licencji maszyny Wirtualnej|03667e82-e009-425a-83f7-8ebddbca5af4|2.166666667|D16s_v3|
|20 procesora wirtualnego vCPU licencji maszyny Wirtualnej|bbd65e5b-35f1-42be-b86d-6625fbc1f1a4|2.166666667||
|24 procesora wirtualnego vCPU licencji maszyny Wirtualnej|c2c07d3e-a7d0-400b-8832-b532bfd0be25|2.166666667|ND24s|
|32 procesorów wirtualnych licencji maszyny Wirtualnej|633d1494-5ec1-46f0-a742-eaf58eeaec7e|2.166666667|D32s_v3|
|40 procesora wirtualnego vCPU licencji maszyny Wirtualnej|737142c3-8e4f-4fc1-aa41-05b1661edff8|2.166666667||
|Procesor wirtualny vCPU 44 licencji maszyny Wirtualnej|722bda73-a8c8-4d04-b96b-541f0bb6c0c4|2.166666667||
|60 procesora wirtualnego vCPU licencji maszyny Wirtualnej|a22bb342-ba9a-4529-a178-39a92ce770b6|2.166666667||
|64 procesory wirtualne licencji maszyny Wirtualnej|d37c8e17-e5f2-4060-881b-080dd4a8c4ce|2.166666667|64s_v3|
|72 procesora wirtualnego vCPU licencji maszyny Wirtualnej|14341b96-e92c-4dca-ba66-322c88a79aa6|2.166666667|F72s_v2|
|96 procesora wirtualnego vCPU licencji maszyny Wirtualnej|8b2e5cb8-0362-4cbf-a30a-115e8d6dbc49|2.166666667||
|128 procesorów wirtualnych licencji maszyny Wirtualnej|9b198a68-974a-47a7-9013-49169ac0f2e9|2.166666667| M128ms|

### <a name="rhel-for-sap-hana"></a>RHEL for SAP HANA

Nazwy witryny portalu Azure marketplace:

- Red Hat Enterprise Linux 6.7 platformy SAP Hana
- Red Hat Enterprise Linux 7.2 for SAP HANA
- Red Hat Enterprise Linux 7.3 platformy SAP Hana

|Red Hat maszyny Wirtualnej | MeterId | Współczynnik|Przykład rozmiar maszyny Wirtualnej|
| ------- |------------------------| --- |--- |
|1 Procesor wirtualny licencji maszyny Wirtualnej|be0a59d1-eed7-47ec-becd-453267753793|1|D2s_v3|
|2 procesory vCPU licencji maszyny Wirtualnej|3b97c9f5-f5d5-4fd3-a421-b78fca32a656|1|D2s_v3|
|4 vCPU licencji maszyny Wirtualnej|b39feb58-57bf-40f2-8193-f4fe9ac3dda3|1|D4s_v3|
|6 procesora wirtualnego vCPU licencji maszyny Wirtualnej|a5963812-0f5a-4053-8ace-2b5babd15ed8|2.166666667||
|8 procesorów wirtualnych licencji maszyny Wirtualnej|5460ab4d-ce9a-46af-8ad5-ca5e53d715b5|2.166666667|D8s_v3|
|12 procesorów wirtualnych licencji maszyny Wirtualnej|0e3bc72d-a888-4bcf-8437-119f763a3215|2.166666667||
|16 vCPU licencji maszyny Wirtualnej|b40e95d8-3176-42f0-967c-497785c031b2|2.166666667|D16s_v3|
|20 procesora wirtualnego vCPU licencji maszyny Wirtualnej|81f34277-499d-40a3-a634-99adc08e2d45|2.166666667||
|24 procesora wirtualnego vCPU licencji maszyny Wirtualnej|e03f1906-d35d-4084-b2cd-63281869c8ee|2.166666667|ND24s|
|32 procesorów wirtualnych licencji maszyny Wirtualnej|0a58c082-ceb8-4327-9b64-887c30dddb23|2.166666667|D32s_v3|
|40 procesora wirtualnego vCPU licencji maszyny Wirtualnej|a14225c0-04e6-4669-974f-e2ddd61a9c5b|2.166666667||
|Procesor wirtualny vCPU 44 licencji maszyny Wirtualnej|378b8125-d8a5-4e09-99bc-c1462534ffb0|2.166666667||
|60 procesora wirtualnego vCPU licencji maszyny Wirtualnej|5d7db11a-54e9-404e-aaa8-509fac7c0638|2.166666667||
|64 procesory wirtualne licencji maszyny Wirtualnej|3c8157b2-a57d-45ce-ba02-bd86e9209795|2.166666667|64s_v3|
|72 procesora wirtualnego vCPU licencji maszyny Wirtualnej|5e87a3ee-7afb-4040-b8d9-b109ddb38f31|2.166666667|F72s_v2|
|96 procesora wirtualnego vCPU licencji maszyny Wirtualnej|b13895fc-0d06-4de9-b860-627c471cd247|2.166666667||
|128 procesorów wirtualnych licencji maszyny Wirtualnej|6e67ac0b-19d3-4289-96df-05d0093d4b3b|2.166666667| M128ms|

## <a name="next-steps"></a>Kolejne kroki

Aby dowiedzieć się więcej na temat rezerwacji, zobacz następujące artykuły:

- [Co to są rezerwacji dla platformy Azure](billing-save-compute-costs-reservations.md)
- [Zapłać z góry za plany oprogramowania Red Hat za pomocą rezerwacji platformy Azure](../virtual-machines/linux/prepay-rhel-software-charges.md)
- [Prepay for Virtual Machines with Azure Reserved VM Instances (Opłacanie maszyn wirtualnych z góry przy użyciu usługi Azure Reserved VM Instances)](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Zarządzanie rezerwacji dla platformy Azure](billing-manage-reserved-vm-instance.md)
- [Opis zastrzeżenia dla Twojej subskrypcji zgodnie z rzeczywistym użyciem](billing-understand-reserved-instance-usage.md)
- [Opis zastrzeżenia dla Twojej rejestracji Enterprise](billing-understand-reserved-instance-usage-ea.md)

## <a name="need-help-contact-us"></a>Potrzebujesz pomocy? Skontaktuj się z nami

Jeśli masz pytania lub potrzebujesz pomocy, [Utwórz żądanie obsługi](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
