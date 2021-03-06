---
ms.date: 06/12/2017
keywords: wmf,powershell,setup
ms.openlocfilehash: 9ead27fd5d4f146e9062488c1c8cc22a073b922e
ms.sourcegitcommit: 54534635eedacf531d8d6344019dc16a50b8b441
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/16/2018
ms.locfileid: "34187098"
---
# <a name="improvements-in-powershell-script-debugging"></a>Vylepšené ladění powershellového skriptu

Množství vylepšení byly provedeny v prostředí PowerShell 5.0 zdokonalování ladění:

## <a name="break-all"></a>Rozdělit všechny

Konzole PowerShell a Windows PowerShell ISE teď umožňují rozdělit na ladicí program ke spouštění skriptů. Toto funguje ve místních i vzdálených relací.

V konzole, stiskněte klávesu **Ctrl + Break**.

V integrovaném Skriptovacím, stiskněte klávesu **Ctrl + B**, nebo použijte **ladění -> Rozdělit všechny** příkazu nabídky.

## <a name="remote-debugging-and-remote-file-editing-in-windows-powershell-ise"></a>Vzdálené ladění a vzdálené úpravy souborů v systému Windows PowerShell ISE

Windows PowerShell ISE nyní umožňuje otevírat a upravovat soubory ve vzdálené relaci spuštěním příkazu PSEdit.
Například můžete otevřít soubor pro úpravy z příkazového řádku v relaci vzdálené následujícím způsobem:

```powershell
[RemoteComputer1]: PS C:\> PSEdit C:\DebugDemoScripts\Test-GetMutex.ps1
```

Kromě toho teď můžete upravit a uložit změny v ke vzdálenému souboru, který se automaticky otevře v systému Windows PowerShell ISE při průchodu zarážky.
Nyní můžete ladění soubor skriptu, který běží na vzdáleném počítači, upravte soubor opravte chybu a pak znovu spusťte upravené skriptu.

## <a name="advanced-script-debugging"></a>Ladění pokročilé skriptů

Existují nové, pokročilé funkce ladění, které umožňují připojení k libovolnému procesu místního počítače, který načetl prostředí Windows PowerShell a ladění libovolný prostředí runspace v tomto procesu.

### <a name="runspace-debugging"></a>Ladění prostředí runspace

Byly přidané nové rutiny, které umožňují seznamu aktuální prostředí runspace v procesu a připojení v konzole pro prostředí Windows PowerShell nebo ISE ladicí program k tohoto prostředí runspace pro ladění skriptů:

-   Get-prostředí Runspace
-   Ladění Runspace
-   Povolit RunspaceDebug
-   Zakázat RunspaceDebug
-   Get-RunspaceDebug

### <a name="attach-to-process-hosting-powershell"></a>Připojit k procesu hostování prostředí PowerShell

Nyní můžete připojit k libovolnému počítače procesu, který má prostředí Windows PowerShell načíst. To uděláte tak, že zadáte do interaktivní relace v procesu, podobně jako na tom, jak zadat do interaktivní vzdálené relace spuštěním rutiny Enter-PSSession:

-   Zadejte PSHostProcess
-   Ukončení PSHostProcess
