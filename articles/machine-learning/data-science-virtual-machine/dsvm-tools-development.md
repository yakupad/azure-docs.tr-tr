---
title: Veri bilimi sanal makine geliştirme araçları - Azure | Microsoft Docs
description: Veri bilimi sanal makine geliştirme araçları.
keywords: Veri bilimi araçları, veri bilimi sanal makine, veri bilimi, linux veri bilimi için Araçlar
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2017
ms.author: gokuma
ms.openlocfilehash: b8b0b8934b51080c3583281673183c1498c26417
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31408318"
---
# <a name="development-tools-on-the-data-science-virtual-machine"></a>Geliştirme araçları üzerinde veri bilimi sanal makine

Veri bilimi sanal makine (DSVM) çeşitli popüler Araçlar ve IDE paketleme, geliştirme için verimli bir ortamı sağlar. Burada, DSVM üzerinde sağlanan bazı araçlar bulunmaktadır. 

## <a name="visual-studio-2017"></a>Visual Studio 2017  
|    |           |
| ------------- | ------------- |
| Nedir?   | Genel amaçlı IDE      |
| Desteklenen DSVM sürümleri      | Windows      |
| Tipik kullanır      | Yazılım geliştirme    |
| Nasıl, yapılandırılmış veya DSVM üzerinde yüklü?      | Veri bilimi iş yükü (Python ve R araçları), Azure iş yükü (Hadoop, Data Lake), Node.js, SQL Server Araçları [AI için Visual Studio Araçları](https://github.com/Microsoft/vs-tools-for-ai)    |
| Kullanın / çalıştırmak için nasıl?      | Masaüstü kısayolu (`C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\Common7\IDE\devenv.exe`)    |
| DSVM ilgili araçları      |     Visual Studio kod, Rstudio'dan, Juno  |

## <a name="visual-studio-code"></a>Visual Studio Code 
|    |           |
| ------------- | ------------- |
| Nedir?   | Genel amaçlı IDE      |
| Desteklenen DSVM sürümleri      | Windows, Linux     |
| Tipik kullanır      | Kod Düzenleyicisi ve Git tümleştirmesi   |
| Kullanın / çalıştırmak için nasıl?      | Masaüstü kısayolu (`C:\Program Files (x86)\Microsoft VS Code\Code.exe`) Windows, Masaüstü kısayoluna veya terminal (`code`) Linux içindeki    |
| DSVM ilgili araçları      |     Visual Studio 2017, Rstudio'dan, Juno  |

## <a name="rstudio--desktop"></a>Rstudio'dan Masaüstü 
|    |           |
| ------------- | ------------- |
| Nedir?   | İstemci IDE r    |
| Desteklenen DSVM sürümleri      | Windows, Linux      |
| Tipik kullanır      |  R geliştirme     |
| Kullanın / çalıştırmak için nasıl?      | Masaüstü kısayolu (`C:\Program Files\RStudio\bin\rstudio.exe`) Windows, masaüstü kısayolu (`/usr/bin/rstudio`) Linux      |
| DSVM ilgili araçları      |   Visual Studio 2017, Visual Studio kodu, Juno      |

## <a name="rstudio--server"></a>RStudio  Server 
|    |           |
| ------------- | ------------- |
| Nedir?   | Web tabanlı IDE r    |
| Desteklenen DSVM sürümleri      | Linux      |
| Tipik kullanır      |  R geliştirme     |
| Kullanın / çalıştırmak için nasıl?      | Hizmet ile etkinleştirmek _systemctl rstudio'dan sunuculu etkinleştirmek_, hizmetle başlar _systemctl Başlat rstudio'dan sunuculu_. Ardından Rstudio'dan sunucuda oturum http://your-vm-ip:8787.       |
| DSVM ilgili araçları      |   Visual Studio 2017, Visual Studio kodu, Rstudio'dan Masaüstü      |

## <a name="juno"></a>Juno 
|    |           |
| ------------- | ------------- |
| Nedir?   | İstemci IDE Jale dil için   |
| Desteklenen DSVM sürümleri      | Windows, Linux      |
| Tipik kullanır      |  Jale geliştirme     |
| Kullanın / çalıştırmak için nasıl?      | Masaüstü kısayolu (`C:\JuliaPro-0.5.1.1\Juno.bat`) Windows, masaüstü kısayolu (`/opt/JuliaPro-VERSION/Juno`) Linux      |
| DSVM ilgili araçları      |   Visual Studio 2017, Visual Studio kodunu Rstudio'dan      |

## <a name="pycharm"></a>Pycharm
|    |           |
| ------------- | ------------- |
| Nedir?   | İstemci IDE Python dil için    |
| Desteklenen DSVM sürümleri      | Linux      |
| Tipik kullanır      |  R geliştirme     |
| Kullanın / çalıştırmak için nasıl?      | Masaüstü kısayolu (`/usr/bin/pycharm`) Linux      |
| DSVM ilgili araçları      |   Visual Studio 2017, Visual Studio kodunu Rstudio'dan      |



## <a name="powerbi-desktop"></a>Desktop'a 
|    |           |
| ------------- | ------------- |
| Nedir?   | Etkileşimli veri Görselleştirme ve BI aracı    |
| Desteklenen DSVM sürümleri      | Windows  |
| Tipik kullanır      |  Veri Görselleştirme ve panolar oluşturma   |
| Kullanın / çalıştırmak için nasıl?      | Masaüstü kısayolu (`C:\Program Files\Microsoft Power BI Desktop\bin\PBIDesktop.exe`)      |
| DSVM ilgili araçları      |   Visual Studio 2017, Visual Studio kodu, Juno      |

