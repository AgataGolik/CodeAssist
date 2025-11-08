# 🧭 FULL GUIDE: CodeAssist on VPS (PuTTY + Vast.ai)

## 1️⃣ REQUIREMENTS

✔️ **VPS (e.g. [Vast.ai](https://vast.ai/))** with:
- at least **4 CPU cores**
- **8 GB RAM**
- **GPU** (e.g. RTX 3060)

✔️ **Ubuntu 22.04** as the operating system  
✔️ **root access via SSH**  
✔️ **PuTTY** installed on your local machine

---
<p align="center" style="color:white; background-color:#ff5555; padding:10px; border-radius:8px;">
⚠️ <b>Do this only if you don’t already have SSH keys</b>
</p>


# 🔹 How to Create an SSH Key with PuTTY and Connect to Vast.ai

## 🔸 Step 1: Generate SSH Keys with PuTTYgen

1. Open **PuTTYgen** (it comes bundled with PuTTY).  
2. Under **Type of key to generate**, choose **RSA** (the most common and secure option).  
3. Click **Generate** and move your mouse around the blank area until the progress bar completes.  
4. Once the key is generated:
   - You’ll see your **public key** in the field labeled **Public key for pasting into OpenSSH authorized_keys file** ->`⚠️ **Copy this – you’ll need it for Vast.ai.**`

   - Set a **key passphrase** if you want extra security and **confirm key passphrase** .
    (When you create your key in PuTTYgen, you can set a passphrase – it’s basically a password that keeps your private key safe.
Each time you restart your computer and open the key file (for example, by double-clicking the CodeAssist.ppk icon on your desktop), PuTTY will ask you to type this passphrase.
You should always enter it before using the key to connect).

![image](https://github.com/AgataGolik/images/blob/main/Projekt%20bez%20nazwy%20(33).png)
![image](https://github.com/AgataGolik/images/blob/main/Zrzut%20ekranu%202025-11-08%20163858.jpg)

6. Then click:
   - **Save private key** → save it as something like `CodeAssist` – this is the file you’ll use in PuTTY.  
   - (You can also click **Save public key**, but copying the text from the box is usually enough.)

## 🔸 Step 2: Upload the Public Key to Vast.ai

1. Log in to [https://vast.ai/console](https://vast.ai/console).  
2. Go to **🔑Keys**.  
3. Click `SSH Key` and `+New`
4. Paste your **public key** from PuTTYgen – it should look something like this:

   `ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQ...`
---

## 2️⃣ CREATE VPS on Vast.ai

Go to [Vast.ai](https://vast.ai/) and create a new instance.  
When selecting your template, switch from the default **NVIDIA CUDA** image to **Ubuntu 22.04 VM**.  
![image](https://github.com/AgataGolik/images/blob/main/Projekt%20bez%20nazwy%20(30).png)
Then choose a machine that meets the requirements (e.g. **RTX 3060**, 4 CPU cores, 8 GB RAM).  
💡 You can often find good offers for around **$0.09/hour**.
![image](https://github.com/AgataGolik/images/blob/main/Projekt%20bez%20nazwy%20(32).png)


## 2️⃣ CONNECT TO VPS VIA SSH (PuTTY)

1. Open **PuTTY** (if you don’t have it yet, download it from [https://www.putty.org/](https://www.putty.org/)).  
2. Fill in the fields (example data below – replace with your own):

| Field | Value |
|-------|--------|
| Host Name | `209.226.130.26` |
| Port | `42812` |
| Connection type | `SSH` |

![image](https://github.com/AgataGolik/images/blob/main/Projekt%20bez%20nazwy%20(35).png)

## 3️⃣ CONFIGURE PORT FORWARDING

In the left menu, go to:  
`Connection → SSH → Tunnels`

Add the following four port forwardings:

| Source port | Destination |
|--------------|--------------|
| 3000 | localhost:3000 |
| 8000 | localhost:8000 |
| 8001 | localhost:8001 |
| 8008 | localhost:8008 |

To add each one:
- enter **Source port**
- enter **Destination**
- select **Local**
- click **Add**

After adding all four, you should see:
L3000 localhost:3000
L8000 localhost:8000
L8001 localhost:8001
L8008 localhost:8008

![image](https://github.com/AgataGolik/images/blob/main/Zrzut%20ekranu%202025-11-08%20170527.jpg)

## 4️⃣ SAVE YOUR SESSION

1. Click **Session** on the left  
2. In **Saved Sessions**, enter a name (e.g. `codeassist-vps`)  
3. Click **Save**

💡 This way you won’t lose your tunnel settings.

![image](https://github.com/AgataGolik/images/blob/main/1.png)

## 5️⃣ CONNECT TO THE VPS

Click **Open** → accept the fingerprint → log in as: `root`
(login: root)

- If you use an SSH key, the connection will start automatically

![image](https://github.com/AgataGolik/images/blob/main/Enter%20the%20passphrase%20you%20created%20earlier%20in%20Step%201%20Generate%20SSH%20Keys%20with%20PuTTYgen.png)


## 6️⃣ INSTALL DEPENDENCIES

Once logged into your VPS terminal, run:

```bash
apt update && apt upgrade -y
apt install screen curl iptables build-essential git wget lz4 jq make gcc nano automake autoconf tmux htop nvme-cli libgbm1 pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip -y
```

## 7️⃣ INSTALL DOCKER
```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh
docker --version
```

✅ Test Docker:
```bash
sudo docker run hello-world
```

## 8️⃣ INSTALL PYTHON AND UV
```bash
apt install python3 python3-pip -y
curl -LsSf https://astral.sh/uv/install.sh | sh
export PATH="/root/.local/bin:$PATH"
uv --version
```

## 9️⃣ CLONE THE CODEASSIST REPOSITORY
```bash
cd ~
git clone https://github.com/gensyn-ai/codeassist.git
cd codeassist
```

## 🔟 RUN CODEASSIST
```bash
uv run run.py
```

At the first launch, you’ll be asked for your HuggingFace token.
Generate one here: https://huggingface.co/settings/tokens

→ Click New Token
→ Name it (e.g. codeassist)
→ Set permission to Write
→ Copy and paste it into the terminal

Wait until everything starts — you’ll see:
`CodeAssist Started
A browser should have opened to http://localhost:3000`

![image](https://github.com/AgataGolik/images/blob/main/Zrzut%20ekranu%202025-11-08%20175102.jpg)

## 1️⃣1️⃣ OPEN CODEASSIST LOCALLY
On your local computer (Windows):
➡️ Open your browser and go to:
`http://localhost:3000`
If everything went well, you’ll see the CodeAssist interface.

Sign in using the same email you always use for the Gensyn dashboard.

![image](https://github.com/AgataGolik/images/blob/main/Zrzut%20ekranu%202025-11-08%20175200.jpg)

![image](https://github.com/AgataGolik/images/blob/main/If%20you%20see%20something%20like%20this%20and%20the%20assistant%20starts%20suggesting%20code%2C%20it%20means%20everything%20is%20working%20correctly..png)

![image](https://github.com/AgataGolik/images/blob/main/Zrzut%20ekranu%202025-11-07%20183717.jpg)

## If you get bored and close PuTTY, don’t worry – nothing happens.
When you log back into your Vast.ai VPS through PuTTY, just run the following commands:

```bash
cd codeassist
```
```bash
uv run run.py
```
➡️ Open your browser and go to:
`http://localhost:3000`

And when you’re ready to jump back in, the fun starts again – you can go back to “solving problems” and hit Submit Solution.

