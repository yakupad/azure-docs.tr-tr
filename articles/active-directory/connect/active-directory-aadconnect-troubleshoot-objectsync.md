---
title: 'Azure AD Connect: Nesne eşitleme sorunlarını giderme | Microsoft Docs'
description: Bu konu ile sorun giderme görevini kullanarak nesne eşitleme sorunlarını gidermek adımlar sağlar.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: curtand
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: f65e84bff63bbdb781991ff6648b0fb98ca5208f
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34592322"
---
# <a name="troubleshoot-object-synchronization-with-azure-ad-connect-sync"></a>Azure AD Connect eşitleme ile nesne eşitleme sorunlarını giderme
Bu makalede, nesne eşitleme sorun giderme görevini kullanarak sorunlarını gidermek için adımları sağlar. Sorun giderme Azure Active Directory (Azure AD) Bağlan nasıl çalıştığını görmek için izleme [bu kısa video](https://aka.ms/AADCTSVideo).

## <a name="troubleshooting-task"></a>Görev sorunlarını giderme
Azure AD için dağıtım 1.1.749.0 sürümüyle bağlanın veya üzeri, sorun giderme görevi nesne eşitleme sorunlarını gidermek için Sihirbazı'nı kullanın. Önceki sürümler için lütfen açıklandığı şekilde el ile ilgili sorunları giderme [burada](active-directory-aadconnectsync-troubleshoot-object-not-syncing.md).

### <a name="run-the-troubleshooting-task-in-the-wizard"></a>Sorun giderme Görev Sihirbazı'nı çalıştırın
Sorun giderme görevi Sihirbazı'nı çalıştırmak için aşağıdaki adımları gerçekleştirin:

1.  Yeni bir Windows PowerShell oturumunda Azure AD Connect sunucunuzda yönetici seçeneği olarak verilerle çalışma açın.
2.  Çalıştırma `Set-ExecutionPolicy RemoteSigned` veya `Set-ExecutionPolicy Unrestricted`.
3.  Azure AD Connect Sihirbazı'nı başlatın.
4.  Ek Görevler sayfasına gidin, sorun giderme seçin ve İleri'yi tıklatın.
5.  Sorun giderme sayfasında, sorun giderme menü PowerShell'de başlatmak için Başlat'ı tıklatın.
6.  Ana menüde nesne eşitleme sorunlarını giderme seçin.
![](media\active-directory-aadconnect-troubleshoot-objectsynch\objsynch11.png)

### <a name="troubleshooting-input-parameters"></a>Sorun giderme giriş parametreleri
Aşağıdaki giriş parametreleri sorun giderme görev tarafından gerekir:
1.  **Nesne ayırt edici adı** – sorun giderme gereken nesnesinin ayırt edici adını budur
2.  **AD Bağlayıcısı adı** – yukarıdaki nesne bulunduğu için AD ormanını adıdır.
3.  Azure AD Kiracı genel yönetici kimlik bilgileri ![](media\active-directory-aadconnect-troubleshoot-objectsynch\objsynch1.png)

### <a name="understand-the-results-of-the-troubleshooting-task"></a>Sorun giderme Görev sonuçlarını anlama
Sorun giderme görev aşağıdaki denetimleri gerçekleştirir:

1.  Nesne Azure Active Directory'ye eşitlenen varsa UPN uyuşmazlığı Algıla
2.  Nesne etki alanı filtreleme nedeniyle filtre uygulanmış olup olmadığını denetleyin
3.  Nesnenin son OU filtreleme için filtre uygulanmış olup olmadığını denetleyin
4.  Nesne eşitleme nedeniyle bağlı bir posta kutusu bloke edilmiş olup olmadığını denetleyin
5. Nesne eşitlenmesi gerektiği dinamik dağıtım grubu olup olmadığını denetleyin

Bu bölümde rest görev tarafından döndürülen belirli sonuçlarını açıklar. Her durumda, görev sorunu gidermek üzere önerilen eylemler tarafından izlenen bir analizini sağlar.

## <a name="detect-upn-mismatch-if-object-is-synced-to-azure-active-directory"></a>Nesne Azure Active Directory'ye eşitlenen varsa UPN uyuşmazlığı Algıla
### <a name="upn-suffix-is-not-verified-with-azure-ad-tenant"></a>UPN soneki ile Azure AD Kiracı doğrulanmadı
Zaman UserPrincipalName (UPN) / alternatif oturum açma Kimliğini soneki ile Azure AD Kiracı doğrulanmadı sonra Azure Active Directory UPN soneki varsayılan etki alanı adı "onmicrosoft.com" ile değiştirir.

![](media\active-directory-aadconnect-troubleshoot-objectsynch\objsynch2.png)

### <a name="changing-upn-suffix-from-one-federated-domain-to-another-federated-domain"></a>UPN soneki bir Federasyon etki alanından başka bir Federasyon etki alanını değiştirme
Azure Active Directory eşitleme, UserPrincipalName (UPN) izin verme / alternatif oturum açma Kimliğini soneki değişiklik bir Federasyon etki alanından başka bir Federasyon etki alanı. Bu, Azure AD Kiracı ile doğrulanır ve federe kimlik doğrulaması türünde olan etki alanları için geçerlidir.

![](media\active-directory-aadconnect-troubleshoot-objectsynch\objsynch3.png) 

### <a name="azure-ad-tenant-dirsync-feature-synchronizeupnformanagedusers-is-disabled"></a>Azure AD Kiracı DirSync özellik 'SynchronizeUpnForManagedUsers' devre dışı bırakıldı
Azure AD Kiracı DirSync özelliği 'SynchronizeUpnForManagedUsers' devre dışı bırakıldığında, Azure Active Directory eşitleme yönetilen kimlik doğrulaması ile lisanslı kullanıcı hesapları için UserPrincipalName/alternatif oturum açma kimliği güncelleştirmeleri izin vermiyor.

![](media\active-directory-aadconnect-troubleshoot-objectsynch\objsynch4.png)

## <a name="object-is-filtered-due-to-domain-filtering"></a>Etki alanı filtreleme nedeniyle nesne filtrelenir
### <a name="domain-is-not-configured-to-sync"></a>Etki alanı yapılandırılmadı eşitleme
Nesne yapılandırılmamış etki alanı nedeniyle kapsamı dışındadır. Ait olduğu etki alanı filtre aşağıdaki örnekte, nesne eşitlenmemiş kapsam aynıdır alanından eşitleme.

![](media\active-directory-aadconnect-troubleshoot-objectsynch\objsynch5.png)

### <a name="domain-is-configured-to-sync-but-is-missing-run-profilesrun-steps"></a>Etki alanı için yapılandırılmış eşitleme ancak çalışma profilleri/çalıştırma adımları eksik
Etki alanı olmadığından nesne kapsam dışında profilleri/çalıştırma adımları çalıştırılır. Ait olduğu etki alanı için tam çalıştırma profili içe çalışma adımları eksik gibi aşağıdaki örnekte eşitlenmemiş kapsam nesnesidir.
![](media\active-directory-aadconnect-troubleshoot-objectsynch\objsynch6.png)

## <a name="object-is-filtered-due-to-ou-filtering"></a>Nesnenin son OU filtreleme için filtrelenir
OU filtreleme yapılandırması nedeniyle eşitlenmemiş kapsam nesnesidir. Aşağıdaki örnekte, nesne OU'ya ait NoSync, DC = bvtadwbackdc, DC = com.  Bu OU eşitleme kapsamında yer almaz.</br>

![OU](media\active-directory-aadconnect-troubleshoot-objectsynch\objsynch7.png)

## <a name="linked-mailbox-issue"></a>Bağlı posta kutusu sorunu
Bağlı bir posta kutusu başka bir güvenilen hesap ormanda bulunan harici bir yönetici hesabı ile ilişkili olduğu varsayılır. Bu tür dış ana hesabı sonra Azure AD Connect değil kullanıcı eşitleme hesabı Azure AD kiracısı için Exchange ormanında bağlı posta kutusu karşılık gelir.</br>
![Bağlı posta kutusu](media\active-directory-aadconnect-troubleshoot-objectsynch\objsynch12.png)

## <a name="dynamic-distribution-group-issue"></a>Dinamik dağıtım grubu sorunu
Şirket içi çeşitli farklılıkları nedeniyle Active Directory ve Azure Active Directory, Azure AD Connect eşitlenmez Azure AD kiracısı dinamik dağıtım gruplarına.

![Dinamik dağıtım grubu](media\active-directory-aadconnect-troubleshoot-objectsynch\objsynch13.png)

## <a name="html-report"></a>HTML raporu
Nesne çözümlemenin yanı sıra, sorun giderme Görev ayrıca her şeyi nesne hakkında bilinen sahip bir HTML raporu oluşturur. Bu HTML raporu yapmak için destek ekibi ile paylaşılabilir daha fazla sorun giderme, gerekirse.

![](media\active-directory-aadconnect-troubleshoot-objectsynch\objsynch8.png)

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md) hakkında daha fazla bilgi edinin.
