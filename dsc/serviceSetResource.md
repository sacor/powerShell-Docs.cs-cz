---
ms.date: 06/12/2017
keywords: DSC prostředí powershell, konfiguraci, instalační program
title: ServiceSet prostředek DSC
ms.openlocfilehash: 1f879d2eafeb11e69968252a11f0c550c9b103f3
ms.sourcegitcommit: 54534635eedacf531d8d6344019dc16a50b8b441
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/16/2018
ms.locfileid: "34188961"
---
# <a name="dsc-serviceset-resource"></a>ServiceSet prostředek DSC

> Platí pro: Prostředí Windows PowerShell 4.0, prostředí Windows PowerShell 5.0


**ServiceSet** prostředků v systému Windows PowerShell požadovaného stavu konfigurace (DSC) poskytuje mechanismus ke správě služeb na cílový uzel. Tento prostředek je [složené prostředků](authoringResourceComposite.md) , který volá [služby prostředků](serviceResource.md) u každé služby zadaný v `Name` vlastnost.

Pokud chcete nakonfigurovat několik služeb do stejného stavu, použijte tento prostředek.

## <a name="syntax"></a>Syntaxe

```
Service [string] #ResourceName
{
    Name = [string[]]
    [ StartupType = [string] { Automatic | Disabled | Manual }  ]
    [ BuiltInAccount = [string] { LocalService | LocalSystem | NetworkService }  ]
    [ State = [string] { Running | Stopped }  ]
    [ Ensure = [string] { Absent | Present }  ]
    [ Credential = [PSCredential] ]
    [ DependsOn = [string[]] ]

}
```

## <a name="properties"></a>Properties

|  Vlastnost  |  Popis   |
|---|---|
| Název| Určuje názvy služeb. Všimněte si, že v některých případech je to jiné názvy zobrazení. Můžete získat seznam služeb a jejich aktuální stav s [Get-Service](https://technet.microsoft.com/library/hh849804.aspx) rutiny.|
| StartupType| Označuje typ spuštění služby. Jsou hodnoty, které jsou povoleny pro tuto vlastnost: **automatické**, **zakázané**, a **ruční**|
| BuiltInAccount| Určuje účet přihlášení, který chcete použít pro služby. Jsou hodnoty, které jsou povoleny pro tuto vlastnost: **LocalService**, **LocalSystem**, a **NetworkService**.|
| Stav| Označuje stav chcete zajistit pro služby: **Zastaveno** nebo **systémem**.|
| Ujistěte se| Udává, zda existuje služeb v systému. Tuto vlastnost nastavit na **chybí** zajistit, že služby nejsou k dispozici. Jeho nastavení na hodnotu **přítomen** (výchozí hodnota) zajišťuje, že existují cíl služby.|
| přihlašovací údaje| Určuje pověření pro účet, který prostředek služby budou spouštěny pod. Tato vlastnost a **BuiltinAccount** vlastnost nelze použít společně.|
| dependsOn| Určuje, že konfigurace jiný prostředek musí spouštět předtím, než je tento prostředek nakonfigurován. Pokud ID konfigurace prostředků skriptu blok, který chcete spustit nejprve je třeba *ResourceName* a její typ je *ResourceType*, syntaxe pro používání této vlastnosti je `DependsOn = "[ResourceType]ResourceName"`.|



## <a name="example"></a>Příklad

Následující konfigurace spuštění služby "Windows Audio" a "Služby Vzdálená plocha".

```powershell
configuration ServiceSetTest
{
    Import-DscResource -ModuleName PSDesiredStateConfiguration
    Node localhost
    {

        ServiceSet ServiceSetExample
        {
            Name        = @("TermService", "Audiosrv")
            StartupType = "Manual"
            State       = "Running"
        }
    }
}
```