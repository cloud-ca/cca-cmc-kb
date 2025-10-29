---
title: "Client Configuration: Windows"
slug: cca-client-config-windows
---

The following instructions apply to Microsoft Windows 10 and 11 using its native VPN client:

#### Add the certificate to the list of trusted certificates

1\. Run `mmc.exe`
2\. Go to *Files > Add/Remove Snap-in…*
3\. Select *Certificates* from the list on the left and click *Add >*.
4\. In the pop-up window, select **Computer account**.

![Certificates snap-in](/assets/Win-1-Computer-Account.png)

5\. In the next window, select **Local Computer: (the computer this console is running on)** and click *Finish*.
6\. Back in the console window, select "*Console Root > Certificates (Local Computer) > Trusted Root Certification Authorities > Certificates*.
7\. Right click on *Certificates* and click *All Tasks > Import…*.

![All tasks menu, import option](/assets/Win-2-Import.png)

8\. In the next window, click *Next*.
9\. Click on *Browse…* and select the certificate file that you saved on disk with the **.crt** extension (the certificate is provided in the Hypertec Cloud UI, in the page to configure the remote management VPN) and click on *Next*.

![Certificate import wizard](/assets/Win-3-Browse.png)

10\. Keep **Place all certificates in the following store: Trusted Root Certification Authorities**  and click *Next*.
11\. Click on *Finish*. You should see the message *The import was successful*. You can close the pop-up and the Console.
12\. If your server is behind a NAT, follow this procedure: [Microsoft link](https://support.microsoft.com/en-us/help/926179/how-to-configure-an-l2tp-ipsec-server-behind-a-nat-t-device-in-windows-vista-and-in-windows-server-2008)

#### Modify the Windows Registry to force strong cipher

**Important:** The change below will be required for the upcoming Cloud Infrastructure update, currently scheduled for November 5, 2025. Please do not apply this change before that date. 

Users on macOS or Linux will not encounter this issue. Those using Windows 10 or Windows 11 must apply this change starting from November 5, 2025 and onward.  By default, Windows 10 and Windows 11 offer a weak cipher when setting up an IKEv2 VPN, which results in a policy mismatch error. To force Windows 10 and Windows 11 to use a stronger cipher the registry requires `DWORD (32bit)NegotiateDH2048_AES256` with value `1` to be added at `Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\RasMan\Parameters`. 

Please note that an upcoming update will require this change to be in place.

**Path:**
```
Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\RasMan\Parameters
```

**Entry:**
```
NegotiateDH2048_AES256 (DWORD 32-bit)
Value: 1
```

The following can be added to a text document and saved with the `.reg` extension and then be doubled clicked to be imported:

```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\RasMan\Parameters]"
NegotiateDH2048_AES256"=dword:00000001
```

#### Create network VPN connection
![Settings](/assets/Win-4-Settings.png)

![VPN Configuration](/assets/Win-5-VPN.png)

![Add Connection](/assets/Win-6-Add-Connection.png)

![Connection Details](/assets/Win-7-Connection-Details.png)

![Select Connection](/assets/Win-8-Select-Connection.png)


#### Initiate VPN connection
![Connect](/assets/Win-9-Connect.png)

![Connected](/assets/Win-10-Connected.png)
