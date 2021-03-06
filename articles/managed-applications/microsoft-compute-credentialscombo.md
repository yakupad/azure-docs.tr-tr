---
title: Azure CredentialsCombo UI öğesi | Microsoft Docs
description: Azure portalı için Microsoft.Compute.CredentialsCombo kullanıcı Arabirimi öğesi açıklar.
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: managed-applications
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: 183075f7407b0a0ca6ea53871e239ab8c2d89490
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37098629"
---
# <a name="microsoftcomputecredentialscombo-ui-element"></a>Microsoft.Compute.CredentialsCombo UI öğesi
Windows ve Linux parolalar ve SSH ortak anahtarları için yerleşik doğrulama denetimleriyle grubudur.

## <a name="ui-sample"></a>Kullanıcı Arabirimi örneği

Windows, kullanıcıları bakın:

![Microsoft.Compute.CredentialsCombo Windows](./media/managed-application-elements/microsoft.compute.credentialscombo-windows.png)

Seçili parola ile Linux için kullanıcıları bakın:

![Microsoft.Compute.CredentialsCombo Linux parola](./media/managed-application-elements/microsoft.compute.credentialscombo-linux-password.png)

Seçili SSH ortak anahtarı ile Linux için kullanıcıları bakın:

![Microsoft.Compute.CredentialsCombo Linux anahtarı](./media/managed-application-elements/microsoft.compute.credentialscombo-linux-key.png)

## <a name="schema"></a>Şema
Windows için aşağıdaki şema kullanın:

```json
{
  "name": "element1",
  "type": "Microsoft.Compute.CredentialsCombo",
  "label": {
    "password": "Password",
    "confirmPassword": "Confirm password"
  },
  "toolTip": {
    "password": ""
  },
  "constraints": {
    "required": true,
    "customPasswordRegex": "^(?=.*[A-Za-z])(?=.*\\d)[A-Za-z\\d]{8,}$",
    "customValidationMessage": "The password must contain at least 8 characters, with at least 1 letter and 1 number."
  },
  "options": {
    "hideConfirmation": false
  },
  "osPlatform": "Windows",
  "visible": true
}
```

İçin **Linux**, aşağıdaki şema kullanın:

```json
{
  "name": "element1",
  "type": "Microsoft.Compute.CredentialsCombo",
  "label": {
    "authenticationType": "Authentication type",
    "password": "Password",
    "confirmPassword": "Confirm password",
    "sshPublicKey": "SSH public key"
  },
  "toolTip": {
    "authenticationType": "",
    "password": "",
    "sshPublicKey": ""
  },
  "constraints": {
    "required": true,
    "customPasswordRegex": "^(?=.*[A-Za-z])(?=.*\\d)[A-Za-z\\d]{8,}$",
    "customValidationMessage": "The password must contain at least 8 characters, with at least 1 letter and 1 number."
  },
  "options": {
    "hideConfirmation": false,
    "hidePassword": false
  },
  "osPlatform": "Linux",
  "visible": true
}
```

## <a name="remarks"></a>Açıklamalar
- `osPlatform` belirtilmeli ve birini kullanabilir **Windows** veya **Linux**.
- Varsa `constraints.required` ayarlanır **doğru**, sonra da parola veya SSH ortak anahtarı metin kutuları başarıyla doğrulamak için değerlere sahip olmalıdır. Varsayılan değer **doğru**.
- Varsa `options.hideConfirmation` ayarlanır **doğru**, sonra da kullanıcının parolasını onayladığınız için ikinci metin kutusu gizli. Varsayılan değer **false**.
- Varsa `options.hidePassword` ayarlanır **doğru**, parola kimlik doğrulaması kullanma seçeneğini gizli sonra. Kullanılabilmesi için yalnızca `osPlatform` olan **Linux**. Varsayılan değer **false**.
- İzin verilen parolalar ek kısıtlamalar kullanarak uygulanabilir `customPasswordRegex` özelliği. Dizede `customValidationMessage` parola özel doğrulama başarısız olduğunda görüntülenir. Her iki özellik için varsayılan değer **null**.

## <a name="sample-output"></a>Örnek çıktı
Varsa `osPlatform` olan **Windows**, veya `osPlatform` olan **Linux** ve kullanıcı tarafından sağlanan parola yerine bir SSH ortak anahtarı, aşağıdaki çıkış denetimi döndürür:

```json
{
  "authenticationType": "password",
  "password": "p4ssw0rd",
}
```

Varsa `osPlatform` olan **Linux** ve kullanıcı tarafından sağlanan bir SSH ortak anahtarı, aşağıdaki çıkış denetimi döndürür:

```json
{
  "authenticationType": "sshPublicKey",
  "sshPublicKey": "AAAAB3NzaC1yc2EAAAABIwAAAIEA1on8gxCGJJWSRT4uOrR13mUaUk0hRf4RzxSZ1zRbYYFw8pfGesIFoEuVth4HKyF8k1y4mRUnYHP1XNMNMJl1JcEArC2asV8sHf6zSPVffozZ5TT4SfsUu/iKy9lUcCfXzwre4WWZSXXcPff+EHtWshahu3WzBdnGxm5Xoi89zcE=",
}
```

## <a name="next-steps"></a>Sonraki adımlar
* UI tanımları oluşturmak için bir giriş için bkz [CreateUiDefinition ile çalışmaya başlama](create-uidefinition-overview.md).
* Kullanıcı Arabirimi öğeleri ortak özellikleri açıklaması için bkz: [CreateUiDefinition öğeleri](create-uidefinition-elements.md).
