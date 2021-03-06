---
title: 'Karma Kimlik: Dizin tümleştirme araçları karşılaştırması | Microsoft Belgeleri'
description: Bu sayfada, dizin tümleştirme için kullanılabilen çeşitli dizin tümleştirme araçlarını karşılaştıran kapsamlı bir tablo sunulmaktadır.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.assetid: 1e62a4bd-4d55-4609-895e-70131dedbf52
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/31/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 10774e7ca1168a58e8c8d47e6a7295ff727fa1cd
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34801458"
---
# <a name="hybrid-identity-directory-integration-tools-comparison"></a>Karma Kimlik dizin tümleştirme araçları karşılaştırması
Yıllar içinde dizin tümleştirme araçları büyüme ve gelişim göstermiştir.  Bu belgenin amacı, bu araçlara yönelik birleştirilmiş bir görünüm ve her birinin içerdiği özelliklere dair bir karşılaştırma sağlamaya yardımcı olmaktır.

<!-- The hardcoded link is a workaround for campaign ids not working in acom links-->

> [!NOTE]
> Azure AD Connect, daha önce Dirsync ve AAD Eşitleme olarak yayınlanan bileşenleri ve işlevleri bir araya getirir. Bu araçlar artık tek tek yayınlanmamaktadır ve ilerideki tüm geliştirmeler Azure AD Connect güncelleştirmelerine dahil edilecektir, böylece her zaman en güncel işlevlere nereden ulaşabileceğinizi bileceksiniz.
> 
> DirSync ve Azure AD Eşitleme kullanım dışı bırakılmıştır. [Buradan](active-directory-aadconnect-dirsync-deprecated.md) daha fazla bilgi edinebilirsiniz.
> 
> 

Tabloların her biri için aşağıdaki anahtarı kullanın.

●  = Şimdi Kullanılabilir  
GS = Gelecekteki Sürüm  
GÖ = Genel Önizleme  

## <a name="on-premises-to-cloud-synchronization"></a>Şirket İçi - Bulut Eşitlemesi
| Özellik | Azure Active Directory Connect | Azure Active Directory Eşitleme Hizmetleri (AAD Eşitleme) - ARTIK DESTEKLENMİYOR | Azure Active Directory Eşitleme Aracı (DirSync) - ARTIK DESTEKLENMİYOR | Forefront Identity Manager 2010 R2 (FIM) | Microsoft Identity Manager 2016 (MIM) |
|:--- |:---:|:---:|:---:|:---:|:---:|
| Tek şirket içi AD ormanına bağlanma |● |● |● |● |● |
| Birden çok şirket içi AD ormanına bağlanma |● |● | |● |● |
| Birden çok şirket içi Exchange Kuruluşu'na bağlanma |● | | | | |
| Tek şirket içi LDAP dizinine bağlanma | | | |● |● |
| Birden çok şirket içi LDAP dizinine bağlanma |  | | |● |● |
| Şirket içi AD ve şirket içi LDAP dizinlerine bağlanma | | | |● |● |
| Özel sistemlere (SQL, Oracle, MySQL vb.) bağlanma |GS | | |● |● |
| Müşteri tanımlı öznitelikleri (dizin uzantıları) eşitleme |● | | | | |
| Şirket içi İK olanağına (SAP, Oracle eBusiness,PeopleSoft) bağlanma |GS | | |● |● |
| FIM eşitleme kurallarını ve şirket içi sistemlere hazırlamaya yönelik bağlayıcıları destekler. | | | |● |● |


## <a name="cloud-to-on-premises-synchronization"></a>Bulut - Şirket İçi Eşitlemesi
| Özellik | Azure Active Directory Connect | Azure Active Directory Eşitleme Hizmetleri - ARTIK DESTEKLENMİYOR  | Azure Active Directory Eşitleme Aracı (DirSync) - ARTIK DESTEKLENMİYOR  | Forefront Identity Manager 2010 R2 (FIM) | Microsoft Identity Manager 2016 (MIM) |
|:--- |:---:|:---:|:---:|:---:|:---:|
| Cihazlar için geri yazma |● | |● | | |
| Öznitelik geri yazma (Exchange karma dağıtımı için) |● |● |● |● |● |
| Gruplar nesneleri için geri yazma |● | | | | |
| Parola geri yazma (self servis parola sıfırlama (SSPR) ve parola değiştirme işleminden) |● |● | | | |

## <a name="authentication-feature-support"></a>Kimlik Doğrulama Özelliği Desteği
| Özellik | Azure Active Directory Connect | Azure Active Directory Eşitleme Hizmetleri - ARTIK DESTEKLENMİYOR  | Azure Active Directory Eşitleme Aracı (DirSync) - ARTIK DESTEKLENMİYOR  | Forefront Identity Manager 2010 R2 (FIM) | Microsoft Identity Manager 2016 (MIM) |
|:--- |:---:|:---:|:---:|:---:|:---:|
| Tek şirket içi AD ormanı için Parola Karma Eşitlemesi |●|●|● | | |
| Birden çok şirket içi AD ormanı için Parola Karma Eşitlemesi |●|● | | | |
| Tek şirket içi AD ormanı için Geçiş Kimlik Doğrulaması |●| | | | |
| Federasyon ile Çoklu Oturum Açma |● |● |● |● |● |
| Sorunsuz Çoklu Oturum Açma|● |||||
| Parola geri yazma (SSPR ve parola değiştirme işleminden) |● |● | | | |

## <a name="set-up-and-installation"></a>Kurulum ve Yükleme
| Özellik | Azure Active Directory Connect | Azure Active Directory Eşitleme Hizmetleri - ARTIK DESTEKLENMİYOR  | Azure Active Directory Eşitleme Aracı (DirSync) - ARTIK DESTEKLENMİYOR  | Microsoft Identity Manager 2016 (MIM) |
|:--- |:---:|:---:|:---:|:---:|
| Etki Alanı Denetleyicisi üzerinde yüklemeyi destekler |● |● |● | |
| SQL Express kullanarak yüklemeyi destekler |● |● |● | |
| DirSync'ten kolay yükseltme |● | | | |
| Yönetici Kullanıcı Deneyiminin Windows Server dillerine yerelleştirmesi |● |● |● | |
| Son kullanıcının kullanıcı deneyiminin Windows Server dillerine yerelleştirmesi | | | |● |
| Windows Server 2008 ve Windows Server 2008 R2 desteği |● Eşitleme içindir, Federasyon için hayır |● |● |● |
| Windows Server 2012 ve Windows Server 2012 R2 desteği |● |● |● |● |

## <a name="filtering-and-configuration"></a>Filtreleme ve Yapılandırma
| Özellik | Azure Active Directory Connect | Azure Active Directory Eşitleme Hizmetleri - ARTIK DESTEKLENMİYOR  | Azure Active Directory Eşitleme Aracı (DirSync) - ARTIK DESTEKLENMİYOR  | Forefront Identity Manager 2010 R2 (FIM) | Microsoft Identity Manager 2016 (MIM) |
|:--- |:---:|:---:|:---:|:---:|:---:|
| Etki Alanları ve Kuruluş Birimleri üzerinde filtreleme |● |● |● |● |● |
| Nesnelerin öznitelik değerleri üzerinde filtreleme |● |● |● |● |● |
| Minimal öznitelik kümesinin eşitlenmesine izin ver (MinSync) |● |● | | | |
| Öznitelik akışları için farklı hizmet şablonlarının uygulanmasına izin ver |● |● | | | |
| Öznitelik kaldırma işleminin AD'den Azure AD'ye akışına izin ver |● |● | | | |
| Öznitelik akışları için gelişmiş özelleştirmeye izin ver |● |● | |● |● |

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md) hakkında daha fazla bilgi edinin.

