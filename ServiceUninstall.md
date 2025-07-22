
### ⚠️ Problem:

* **Elastic Agent service exists**
* **Install path is missing** (`C:\Program Files\Elastic\Agent`)
* So **installer can't overwrite or uninstall the broken agent**
* You're stuck in a “half-installed” state.

---

## ✅ Goal: Clean broken Elastic Agent so it can be reinstalled

Here’s the **cleanest, download-free fix** using only **built-in tools**:

---

## 🛠️ Step-by-Step Fix (No Tools Needed)

---

### 🔹 Step 1: Stop and Delete the Service (again)

Open **Command Prompt as Administrator**:

````cmd
sc stop "Elastic Agent"
sc delete "Elastic Agent"
``>

- If you get `service does not exist`, skip to Step 2
- If you get `access denied`, let me know — we’ll bypass that

---

### 🔹 Step 2: Delete the registry key

Open **PowerShell as Administrator**, run:
```powershell
$svc = "Elastic Agent"
$key = "HKLM:\SYSTEM\CurrentControlSet\Services\elastic-agent"

# Take ownership and permissions
Start-Process powershell -Verb runAs -ArgumentList @"
takeown /f `"HKLM\SYSTEM\CurrentControlSet\Services\elastic-agent`" /a /r /d y
icacls `"HKLM\SYSTEM\CurrentControlSet\Services\elastic-agent`" /grant administrators:F /t
"@

# Delete the key
Remove-Item -Path $key -Recurse -Force -ErrorAction SilentlyContinue
````

---

### 🔹 Step 3: Recreate the missing folder

Even though it’s broken, the installer **wants `C:\Program Files\Elastic\Agent` to exist** to proceed cleanly.

So manually **recreate the empty folder**:

```powershell
New-Item -ItemType Directory -Path "C:\Program Files\Elastic\Agent" -Force
```

---

### 🔹 Step 4: Run the installer again

Now that the service is deleted and the folder exists, try again:

```cmd
.\ElasticAgentInstaller.msi
```

It should now successfully **reinstall cleanly** over the stubbed directory.

---

## ✅ After Reinstall:

* Open `services.msc` — you should see **Elastic Agent** running
* Confirm install path is populated
* Elastic Agent will now be manageable via Fleet or CLI

---

