---
ms.date: 06/05/2017
keywords: rutiny prostředí PowerShell
title: Vytvoření vlastního vstupního pole
ms.assetid: 0b12e56c-299f-40ee-afbf-d30d23ed2565
ms.openlocfilehash: 681a75a28a8fb03eb4442d5e20b32b25a337d540
ms.sourcegitcommit: cf195b090b3223fa4917206dfec7f0b603873cdf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/09/2018
ms.locfileid: "30954751"
---
# <a name="creating-a-custom-input-box"></a>Vytvoření vlastního vstupního pole

Skript grafické vlastní vstupní pole s použitím funkcí rozhraní Microsoft .NET Framework vytváření formulářů v prostředí Windows PowerShell 3.0 a novějších verzí.

## <a name="create-a-custom-graphical-input-box"></a>Vytvoření vlastní, grafické vstupní pole

Zkopírujte a vložte následující do systému Windows PowerShell ISE a pak ho uložte jako skript prostředí Windows PowerShell (.ps1).

```powershell
Add-Type -AssemblyName System.Windows.Forms
Add-Type -AssemblyName System.Drawing

$form = New-Object System.Windows.Forms.Form
$form.Text = 'Data Entry Form'
$form.Size = New-Object System.Drawing.Size(300,200)
$form.StartPosition = 'CenterScreen'

$OKButton = New-Object System.Windows.Forms.Button
$OKButton.Location = New-Object System.Drawing.Point(75,120)
$OKButton.Size = New-Object System.Drawing.Size(75,23)
$OKButton.Text = 'OK'
$OKButton.DialogResult = [System.Windows.Forms.DialogResult]::OK
$form.AcceptButton = $OKButton
$form.Controls.Add($OKButton)

$CancelButton = New-Object System.Windows.Forms.Button
$CancelButton.Location = New-Object System.Drawing.Point(150,120)
$CancelButton.Size = New-Object System.Drawing.Size(75,23)
$CancelButton.Text = 'Cancel'
$CancelButton.DialogResult = [System.Windows.Forms.DialogResult]::Cancel
$form.CancelButton = $CancelButton
$form.Controls.Add($CancelButton)

$label = New-Object System.Windows.Forms.Label
$label.Location = New-Object System.Drawing.Point(10,20)
$label.Size = New-Object System.Drawing.Size(280,20)
$label.Text = 'Please enter the information in the space below:'
$form.Controls.Add($label)

$textBox = New-Object System.Windows.Forms.TextBox
$textBox.Location = New-Object System.Drawing.Point(10,40)
$textBox.Size = New-Object System.Drawing.Size(260,20)
$form.Controls.Add($textBox)

$form.Topmost = $true

$form.Add_Shown({$textBox.Select()})
$result = $form.ShowDialog()

if ($result -eq [System.Windows.Forms.DialogResult]::OK)
{
    $x = $textBox.Text
    $x
}
```

Zahájí skript načtením dvě třídy rozhraní .NET Framework: **System.Drawing** a **System.Windows.Forms**. Spusťte novou instanci třídy rozhraní .NET Framework **formuláře System.Windows.Forms.Form**; který poskytuje prázdného formuláře nebo okna, do kterého můžete začít přidávat ovládací prvky.

```powershell
$form = New-Object System.Windows.Forms.Form
```

Po vytvoření instance třídy formuláře, hodnoty přiřadíte tři vlastnosti této třídy.

- **Text.** To všechno bude v záhlaví okna.

- **Velikost.** Toto je velikost formuláře v pixelech. Předchozí skript vytvoří formulář, který je 300 × široké 200 pixelů vysoký.

- **StartingPosition.** Tato volitelná vlastnost nastavena na **CenterScreen** v předchozí skript. Pokud přidáte nemusíte tuto vlastnost, systém Windows vybere umístění při otevření formuláře. Nastavením **StartingPosition** k **CenterScreen**, automaticky zobrazujete formuláře uprostřed obrazovky pokaždé, když ho načte.

```powershell
$form.Text = 'Data Entry Form'
$form.Size = New-Object System.Drawing.Size(300,200)
$form.StartPosition = 'CenterScreen'
```

Dále vytvořte **OK** tlačítko pro daný formulář. Zadejte velikost a chování **OK** tlačítko. V tomto příkladu je pozice tlačítko 120 pixelů od horního okraje formuláře a 75 pixelů od levého okraje. Výška tlačítko je 23 pixelů, zatímco tlačítko délka je 75 pixelů. Skript používá předdefinovaných typů Windows Forms k určení chování tlačítko.

```powershell
$OKButton = New-Object System.Windows.Forms.Button
$OKButton.Location = New-Object System.Drawing.Point(75,120)
$OKButton.Size = New-Object System.Drawing.Size(75,23)
$OKButton.Text = 'OK'
$OKButton.DialogResult = [System.Windows.Forms.DialogResult]::OK
$form.AcceptButton = $OKButton
$form.Controls.Add($OKButton)
```

Podobně můžete vytvořit **zrušit** tlačítko. **Zrušit** tlačítko je 120 pixelů z horní části, ale 150 pixelů od levého okraje okna.

```powershell
$CancelButton = New-Object System.Windows.Forms.Button
$CancelButton.Location = New-Object System.Drawing.Point(150,120)
$CancelButton.Size = New-Object System.Drawing.Size(75,23)
$CancelButton.Text = 'Cancel'
$CancelButton.DialogResult = [System.Windows.Forms.DialogResult]::Cancel
$form.CancelButton = $CancelButton
$form.Controls.Add($CancelButton)
```

Potom zadejte text popisku na vaše okno, které popisuje informace, které chcete, aby uživatelé zadali.

```powershell
$label = New-Object System.Windows.Forms.Label
$label.Location = New-Object System.Drawing.Point(10,20)
$label.Size = New-Object System.Drawing.Size(280,20)
$label.Text = 'Please enter the information in the space below:'
$form.Controls.Add($label)
```

Přidání ovládacího prvku (v tomto případě textového pole), která umožňuje uživatelům poskytnout informace, které jste popsaných v textu popisku. Existuje mnoho dalších ovládacích prvků, které můžete použít kromě polí; pro další ovládací prvky, najdete v části [System.Windows.Forms Namespace](http://msdn.microsoft.com/library/k50ex0x9(v=vs.110).aspx) na webu MSDN.

```powershell
$textBox = New-Object System.Windows.Forms.TextBox
$textBox.Location = New-Object System.Drawing.Point(10,40)
$textBox.Size = New-Object System.Drawing.Size(260,20)
$form.Controls.Add($textBox)
```

Nastavte **Topmost** vlastnost **$true** vynutit okno Otevřít na otevřená okna a dialogová okna.

```powershell
$form.Topmost = $true
```

V dalším kroku přidejte tento řádek kód pro aktivaci formuláře a nastavení fokusu textové pole, které jste vytvořili.

```powershell
$form.Add_Shown({$textBox.Select()})
```

Přidejte následující řádek kódu k zobrazení formuláře v systému Windows.

```powershell
$result = $form.ShowDialog()
```

Nakonec se kód uvnitř **Pokud** bloku dá pokyn Windows co dělat s formulářem po uživatele zadejte text do textového pole a pak klikněte na tlačítko **OK** tlačítko nebo klikněte na tlačítko **Enter** klíč.

```powershell
if ($result -eq [System.Windows.Forms.DialogResult]::OK)
{
    $x = $textBox.Text
    $x
}
```

## <a name="see-also"></a>Viz také

- [Hey Scripting Guy: Proč se nepodporují tyto příklady prostředí PowerShell grafického uživatelského rozhraní?](http://go.microsoft.com/fwlink/?LinkId=506644)
- [Githubu: WinFormsExampleUpdates Dave Wyatt](https://github.com/dlwyatt/WinFormsExampleUpdates)
- [Windows PowerShell Tip v týdnu: vytvoření vlastní vstupní pole](http://technet.microsoft.com/library/ff730941.aspx)