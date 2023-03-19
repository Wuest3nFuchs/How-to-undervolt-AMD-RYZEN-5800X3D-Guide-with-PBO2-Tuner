How to "undervolt" AMD RYZEN 5800X3D Guide with PBO2 tuner.

Get the Most out of your 5800X3D using PBO curve optimizer!


# Table of Contents
1. [What are we doing?](#1-what-are-we-doing)
2. [Why are we doing it? What are the Benefits?](#2-why-are-we-doing-it-what-are-the-benefits)
3. [Options on what we can do](#3-options-on-what-we-can-do)
4. [What do we need?](#4-what-do-we-need)
5. [How to set PBO2 Tuner manually](#5-how-to-set-pbo2-tuner-manually)
6. [How to set PBO2 Tuner automatically with every System boot/restart/wakeup](#6-how-to-set-pbo2-tuner-automatically-with-every-system-bootrestartwakeup)
7. [(Optional) How to create a custom Alert for every time your PBO2 Tuner settings have been applied automatically](#7-optional-how-to-create-a-custom-alert-for-every-time-your-pbo2-tuner-settings-have-been-applied-automatically)
8. [What else can we do to improve CPU Clockspeeds and Thermals?](#8-what-else-can-we-do-to-improve-cpu-clockspeeds-and-thermals)
9.  [Resources used](#9-resources-used)


# 1) What are we doing? 
We are using PBO2 tuner to set an OC curve (AMD curve optimizer)

# 2) Why are we doing it? What are the Benefits?
Because the 5800X3D is very much locked down on most Motherboards thanks to AMD and the Motherboard Manufacturers, most people won't be able to set CPU Ratio and/or CPU Voltage in their Bios to get the most out of their 5800X3D.
With this Guide, you will at least be able to tweak Things a little inside of Windows for potentially better Thermals and Clockspeeds that stay longer and more often at the highest they can be.

# 3) Options on what we can do
Set PBO OC Curve manually after every change in Windows powerstate
     
Set PBO OC Curve automatically with every system boot/restart/wakeup (means after every change in Windows powerstate)
     
**Optional** possibility to create custom BurnedToast Alert whenever the CO Curve has been set automatically. This way, you will always have the assurance that it actually applied.
    
# 4) What do we need?
[Debug-cli.7zp](https://www.overclock.net/threads/corecycler-tool-for-testing-curve-optimizer-settings.1777398/page-45#post-28999750) from PJVol from Overclocker.net thread

Monitoring software to confirm CO Curve has been set([HwInfo](https://www.hwinfo.com/download/) or [RyzenMaster](https://www.amd.com/en/technologies/ryzen-master))

BurntToast Module in PowerShell

Allow Execution Policies change in PowerShell

PowerShell script for BurntToast Alert

Any PNG of your Choice for the Alert

# 5) How to set PBO2 Tuner manually
First download [Debug-cli.7zp](https://www.overclock.net/threads/corecycler-tool-for-testing-curve-optimizer-settings.1777398/page-45#post-28999750).

Get a Monitoring Software like [HwInfo](https://www.hwinfo.com/download/) or [RyzenMaster](https://www.amd.com/en/technologies/ryzen-master). You will need this to see your CPU Voltage and Temperature and to later confirm that your Settings work.

Run a CPU Stress Test like OCCT or Benchmark like Cinebench R20/R23 to push your CPU to ~100% Utilization. See how high your CPU boosts and what Temperatures you get before any changes to it.

Now open PBO2 Tuner.exe from the Folder we just downloaded and select the Curve tab

Start by setting a low negative offset like -10 and decrease it in decrements of -5 to a Minimum of -30.

Make sure to stress test it each step to see if it is stable.

Now you're done! But remember, you will have to do this again every time you restart or wake from sleep.

# 6) How to set PBO2 Tuner automatically with every System boot/restart/wakeup
Do all the Steps in [Chapter 5](#5-how-to-set-pbo2-tuner-manually) until you're satisfied with the offsets you can get.

Now Open Windows Task Scheduler. Go to Actions and Create Task.

**Use the provided Screenshots to configure the Task correctly.**

Set General Settings.

![Chapter 6 p1_LI](https://user-images.githubusercontent.com/106172193/170519308-df13b85a-00c8-4e57-ad12-6d9b118ed1ac.jpg)

Create the Trigger for System startup.

![Chapter 6 p2](https://user-images.githubusercontent.com/106172193/170519471-b033ca84-48ea-4822-88a4-a9daecc33b7f.png)

Create the Trigger for Wakeup.

![Chapter 6 p3](https://user-images.githubusercontent.com/106172193/170519673-90d6766b-6397-4638-a2b8-08c8028839e9.png)

Create the Action for PBO2 tuner.exe to start with the Offsets as Startup arguments.

![Chapter 6 p4_LI](https://user-images.githubusercontent.com/106172193/170519780-4835e046-82fc-49fe-8a9f-e1ded1144638.jpg)

Set the Conditions and Settings tab.
Applies a CO [Curve Optimizer] offset of -30[~150mV] to all cores

Arguments
-30 -30 -30 -30 -30 -30 -30 -30 95 60 90 0

-30 is for the curve optimizer -95=PPT -60=TDC -90=EDC 0= FIT scalar

![Chapter 6 p5](https://user-images.githubusercontent.com/106172193/170519955-8fd6d69b-36ec-4e1c-b7b8-906fda4a28ac.png)

![Chapter 6 p6](https://user-images.githubusercontent.com/106172193/170520002-83e3a008-9177-4a35-98be-f5005b559db9.png)


**Make sure to set the path to your PBO2 tuner.exe since it most likely won't be the same as mine.**

**Customize the Arguments to your previously tested Offsets.**

Click OK to save it. Congratulations, you are done!

# 7) (Optional) How to create a custom Alert for every time your PBO2 Tuner settings have been applied automatically
If you are as paranoid as me and have the urge to check PBO2 Tuner after every reboot or wakeup to see if your offset has been set, then I have something to ease your Mind. A Custom Alert tied to the Task Scheduler we just created above to inform you that the Task has been executed.

![Chapter 7 p1](https://user-images.githubusercontent.com/106172193/170520421-a21eec68-2311-4eb7-ad44-d695410b9a1c.png)

Find a Logo and save into the folder with PBO2 tuner.exe in it.

Open a Text file and paste this into it:

```pwsh
Import-Module BurntToast;
$pngPath="C:\Users\REPLACE_THIS_WITH_THE_PATH_TO_THE_PNG_FILE\whatever.png";
New-BurntToastNotification -Text "PBO Offset has been applied!", 'PBO Offset has been applied!' -AppLogo $pngPath
```

Save as `Alert.ps1`, select All Files types and save in the same folder as PBO2 tuner.exe.

Open Windows PowerShell as Administrator and execute this command:

```pwsh
Install-Module -Name BurntToast
```

You will, likely, get a message that says you need to install the NuGet provider. If you do, simply type in Y to proceed and PowerShell will take care of the rest. Once it’s installed, run the above command again. This time, you will likely get a message saying you’re installing a module from an untrusted repository. Again, type Y to proceed. The module will now be installed.

Now paste

```pwsh
Set-ExecutionPolicy -ExecutionPolicy Unrestricted
```

into PowerShell and run the command. Press Y to proceed. This is necessary for `Alert.ps1` to work in the future.

**DO THIS AT YOUR OWN RISK**, since you're disabling a security feature of windows.

Now Open Task Scheduler again and look for the Task we've created in [Chapter 6](#6-how-to-set-pbo2-tuner-automatically-with-every-system-bootrestartwakeup).

Right click it, select Properties, go to Actions and click New.

**Use the provided Screenshot to set up correctly.**

![Chapter 7 p2_LI](https://user-images.githubusercontent.com/106172193/170520568-ed331286-d0be-4b65-9539-7949ad0334ce.jpg)

**Edit Add arguments to the path of your Alert.ps1 file**

Click okay and save all changes.

We're done!
    
You can test it by right-clicking the Task in Task Scheduler and selecting run.

# 8) What else can we do to improve CPU Clockspeeds and Thermals?
Pretty much nothing without the support from AMD and or Motherboard Manufacturers.

Get a better CPU cooler.

(For Enthusiasts with an AIO or Custom Water cooling Block) Get an AM4 Cooler offset bracket(like from DerBauer); reportedly around 5-10 degrees improvement on Core temps

Set higher Baseclock (really not recommended if you don't exactly know what it does and what Problems can be caused by it)
# 9) Resources used
- https://www.overclock.net/threads/corecycler-tool-for-testing-curve-optimizer-settings.1777398/
- https://www.overclock.net/threads/5800x3d-owners.1798046/




------------------------------------------------------------------------Deutsche Übersetzung-------------------------------------------------------------


Wie man AMD RYZEN 5800X3D mit dem PBO2 Tuner "undervolted".

Holen Sie das Beste aus Ihrem 5800X3D mit dem PBO Kurvenoptimierer heraus!


# Inhaltsverzeichnis
1. [Was machen wir?](#1-was-machen-wir)
2. [Warum tun wir es? Was sind die Vorteile?](#2-warum-tun-wir-es-was-sind-die-Vorteile)
3. [Optionen, was wir tun können](#3-optionen-auf-was-wir-tun-können)
4. [Was brauchen wir?](#4-was-brauchen-wir)
5. [Wie man den PBO2-Tuner manuell einstellt](#5-how-to-set-pbo2-tuner-manually)
6. [PBO2 Tuner automatisch bei jedem Systemstart einstellen](#6-how-to-set-pbo2-tuner-automatically-with-every-system-boot-restartwakeup)
7. [(Optional) So erstellen Sie eine benutzerdefinierte Warnung für jedes Mal, wenn Ihre PBO2-Tuner-Einstellungen automatisch angewendet wurden](#7-optional-how-to-create-a-custom-alert-for-every-time-your-pbo2-tuner-settings-have-been-applied-automatically)
8. [Was können wir noch tun, um die CPU-Taktfrequenz und die Thermik zu verbessern?](#8-was-können-wir-noch-tun-um-cpu-taktfrequenz-und-thermik-zu-verbessern)
9.  [Verwendete Ressourcen](#9-resources-used)


# 1) Was machen wir? 
Wir verwenden den PBO2-Tuner, um eine OC-Kurve einzustellen (AMD curve optimizer)

# 2) Warum tun wir das? Was sind die Vorteile?
Da die 5800X3D auf den meisten Mainboards dank AMD und den Mainboardherstellern fest verbaut ist, werden die meisten Leute nicht in der Lage sein, CPU Ratio und/oder CPU Voltage in ihrem Bios einzustellen, um das Beste aus ihrer 5800X3D herauszuholen.
Mit diesem Leitfaden wirst du zumindest in der Lage sein, die Dinge innerhalb von Windows ein wenig zu optimieren, um potentiell bessere Thermik und Taktraten zu erreichen, die länger und öfter auf dem höchstmöglichen Niveau bleiben.

# 3) Optionen, was wir tun können
PBO OC Curve manuell nach jeder Änderung im Windows Powerstate einstellen
     
PBO OC Curve automatisch bei jedem Systemstart/Restart/Wakeup einstellen (d.h. nach jeder Änderung des Windows Powerstates)
     
**Optional** Möglichkeit, einen benutzerdefinierten BurnedToast Alert zu erstellen, wenn die CO-Kurve automatisch eingestellt wurde. Auf diese Weise haben Sie immer die Gewissheit, dass sie tatsächlich angewendet wurde.
    
# 4) Was brauchen wir?
[Debug-cli.7zp](https://www.overclock.net/threads/corecycler-tool-for-testing-curve-optimizer-settings.1777398/page-45#post-28999750) von PJVol aus dem Overclocker.net Thread

Überwachungssoftware zur Bestätigung, dass die CO-Kurve eingestellt wurde ([HwInfo](https://www.hwinfo.com/download/) oder [RyzenMaster](https://www.amd.com/en/technologies/ryzen-master))

BurntToast-Modul in PowerShell

Änderung der Ausführungsrichtlinien in PowerShell zulassen

PowerShell-Skript für BurntToast-Warnung

Ein beliebiges PNG Ihrer Wahl für den Alert

# 5) Wie man PBO2 Tuner manuell einstellt
Laden Sie zunächst [Debug-cli.7zp] herunter (https://www.overclock.net/threads/corecycler-tool-for-testing-curve-optimizer-settings.1777398/page-45#post-28999750).

Besorgen Sie sich eine Überwachungssoftware wie [HwInfo](https://www.hwinfo.com/download/) oder [RyzenMaster](https://www.amd.com/en/technologies/ryzen-master). Sie benötigen diese, um Ihre CPU-Spannung und Temperatur zu sehen und um später zu bestätigen, dass Ihre Einstellungen funktionieren.

Führen Sie einen CPU-Stresstest wie OCCT oder einen Benchmark wie Cinebench R20/R23 durch, um Ihre CPU auf ~100% Auslastung zu bringen. Beobachten Sie, wie hoch Ihre CPU hochgefahren wird und welche Temperaturen Sie erhalten, bevor Sie etwas daran ändern.

Öffnen Sie nun PBO2 Tuner.exe aus dem Ordner, den wir gerade heruntergeladen haben, und wählen Sie die Registerkarte Kurve

Beginnen Sie mit einem niedrigen negativen Offset wie -10 und verringern Sie ihn in Schritten von -5 bis zu einem Minimum von -30.

Stellen Sie sicher, dass Sie bei jedem Schritt einen Stresstest durchführen, um zu sehen, ob er stabil ist.

Jetzt sind Sie fertig! Aber denken Sie daran, dass Sie dies jedes Mal wiederholen müssen, wenn Sie neu starten oder aus dem Schlaf erwachen.

# 6) Wie man den PBO2 Tuner automatisch bei jedem Systemstart/Reboot/Wakeup einstellt
Führen Sie alle Schritte in [Kapitel 5](#5-how-to-set-pbo2-tuner-manually) aus, bis Sie mit den erzielten Offsets zufrieden sind.

Öffnen Sie nun den Windows-Taskplaner. Gehen Sie zu Aktionen und Task erstellen.

**Verwenden Sie die mitgelieferten Screenshots, um den Task korrekt zu konfigurieren.

Legen Sie die allgemeinen Einstellungen fest.

![Chapter 6 p1_LI](https://user-images.githubusercontent.com/106172193/170519308-df13b85a-00c8-4e57-ad12-6d9b118ed1ac.jpg)

Erstellen Sie den Auslöser für den Systemstart.

![Chapter 6 p2](https://user-images.githubusercontent.com/106172193/170519471-b033ca84-48ea-4822-88a4-a9daecc33b7f.png)

Erstellen Sie den Auslöser für das Aufwachen.

![Chapter 6 p3](https://user-images.githubusercontent.com/106172193/170519673-90d6766b-6397-4638-a2b8-08c8028839e9.png)

Erstellen Sie die Aktion für PBO2 tuner.exe, um mit den Offsets als Startargumente zu starten.

![Chapter 6 p4_LI](https://user-images.githubusercontent.com/106172193/170519780-4835e046-82fc-49fe-8a9f-e1ded1144638.jpg)

Legen Sie die Registerkarte Bedingungen und Einstellungen fest.
Wendet einen CO [Curve Optimizer] Offset von -30[~150mV] auf alle Kerne an

Argumente
-30 -30 -30 -30 -30 -30 -30 -30 95 60 90 0

-30 ist für den Kurvenoptimierer -95=PPT -60=TDC -90=EDC 0= FIT-Skalar

![Chapter 6 p5](https://user-images.githubusercontent.com/106172193/170519955-8fd6d69b-36ec-4e1c-b7b8-906fda4a28ac.png)

![Chapter 6 p6](https://user-images.githubusercontent.com/106172193/170520002-83e3a008-9177-4a35-98be-f5005b559db9.png)


**Stellen Sie sicher, dass Sie den Pfad zu Ihrer PBO2 tuner.exe angeben, da er höchstwahrscheinlich nicht derselbe ist wie meiner.

**Passen Sie die Argumente auf Ihre zuvor getesteten Offsets an.

Klicken Sie auf OK, um die Datei zu speichern. Herzlichen Glückwunsch, Sie sind fertig!

# 7) (Optional) So erstellen Sie eine benutzerdefinierte Warnung für jedes Mal, wenn Ihre PBO2-Tuner-Einstellungen automatisch angewendet wurden
Wenn Sie so paranoid sind wie ich und den Drang haben, PBO2 Tuner nach jedem Neustart oder Aufwachen zu überprüfen, um zu sehen, ob Ihr Offset eingestellt wurde, dann habe ich etwas, das Sie beruhigen wird. Eine benutzerdefinierte Warnung, die mit dem oben erstellten Taskplaner verknüpft ist und Sie darüber informiert, dass der Task ausgeführt wurde.

![Chapter 7 p1](https://user-images.githubusercontent.com/106172193/170520421-a21eec68-2311-4eb7-ad44-d695410b9a1c.png)

Suchen Sie ein Logo und speichern Sie es in dem Ordner, in dem sich PBO2 tuner.exe befindet.

Öffnen Sie eine Textdatei und fügen Sie den folgenden Text ein:

```pwsh
Importiere-Modul BurntToast;
$pngPath="C:\Users\REPLACE_THIS_WITH_THE_PATH_TO_THE_PNG_FILE\whatever.png";
New-BurntToastNotification -Text "PBO-Offset wurde angewendet!", 'PBO-Offset wurde angewendet!' -AppLogo $pngPath
```

Speichern Sie als "Alert.ps1", wählen Sie "Alle Dateitypen" aus und speichern Sie im selben Ordner wie "PBO2 tuner.exe".

Öffnen Sie Windows PowerShell als Administrator und führen Sie diesen Befehl aus:

```pwsh
Install-Module -Name BurntToast
```

Wahrscheinlich werden Sie eine Meldung erhalten, dass Sie den NuGet-Anbieter installieren müssen. Wenn dies der Fall ist, geben Sie einfach Y ein, um fortzufahren, und PowerShell kümmert sich um den Rest. Nach der Installation führen Sie den obigen Befehl erneut aus. Diesmal werden Sie wahrscheinlich eine Meldung erhalten, dass Sie ein Modul aus einem nicht vertrauenswürdigen Repository installieren. Geben Sie erneut Y ein, um fortzufahren. Das Modul wird nun installiert.

Fügen Sie nun ein

``pwsh
Set-ExecutionPolicy -ExecutionPolicy Unrestricted
```

in die PowerShell ein und führen Sie den Befehl aus. Drücken Sie Y, um fortzufahren. Dies ist notwendig, damit `Alert.ps1` in Zukunft funktioniert.

**TUN SIE DIES AUF IHR EIGENES RISIKO**, da Sie damit eine Sicherheitsfunktion von Windows deaktivieren.

Öffnen Sie nun wieder den Taskplaner und suchen Sie den Task, den wir in [Kapitel 6](#6-how-to-set-pbo2-tuner-automatically-with-every-system-bootrestartwakeup) erstellt haben.

Klicken Sie mit der rechten Maustaste darauf, wählen Sie Eigenschaften, gehen Sie zu Aktionen und klicken Sie auf Neu.

**Verwenden Sie den mitgelieferten Screenshot, um die korrekte Einstellung vorzunehmen.

![Chapter 7 p2_LI](https://user-images.githubusercontent.com/106172193/170520568-ed331286-d0be-4b65-9539-7949ad0334ce.jpg)

**Bearbeiten Sie Argumente zum Pfad Ihrer Datei Alert.ps1 hinzufügen**

Klicken Sie auf OK und speichern Sie alle Änderungen.

Das war's!
    
Sie können den Task testen, indem Sie im Taskplaner mit der rechten Maustaste auf den Task klicken und Ausführen wählen.

# 8) Was können wir noch tun, um die CPU-Taktraten und die Thermik zu verbessern?
So gut wie nichts ohne die Unterstützung von AMD und den Motherboard-Herstellern.

Besorgen Sie sich einen besseren CPU-Kühler.

(Für Enthusiasten mit einem AIO oder benutzerdefinierten Wasserkühlungsblock) Holen Sie sich einen AM4-Kühler Offset-Bracket (wie von DerBauer); Berichten zufolge etwa 5-10 Grad Verbesserung auf Core-Temperaturen

Stellen Sie eine höhere Baseclock ein (wirklich nicht empfohlen, wenn Sie nicht genau wissen, was sie tut und welche Probleme dadurch verursacht werden können)
# 9) Verwendete Ressourcen
- https://www.overclock.net/threads/corecycler-tool-for-testing-curve-optimizer-settings.1777398/
- https://www.overclock.net/threads/5800x3d-owners.1798046/

Übersetzt mit www.DeepL.com/Translator (kostenlose Version)
