# 🧭 FULL GUIDE: CodeAssist on VPS (PuTTY + Vast.ai)

## 1️⃣ REQUIREMENTS

✔️ **VPS (e.g. [Vast.ai](https://vast.ai/))** with:
- at least **4 CPU cores**
- **8 GB RAM**
- **GPU** (e.g. RTX 3060)

✔️ **Ubuntu 22.04** as the operating system  
✔️ **root access via SSH**  
✔️ **PuTTY** installed on your local machine

## 2️⃣ CONNECT TO VPS VIA SSH (PuTTY)

1. Open **PuTTY** (if you don’t have it yet, download it from [https://www.putty.org/](https://www.putty.org/)).  
2. Fill in the fields (example data below – replace with your own):

| Field | Value |
|-------|--------|
| Host Name | `209.226.130.26` |
| Port | `42812` |
| Connection type | `SSH` |

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


## 4️⃣ SAVE YOUR SESSION

1. Click **Session** on the left  
2. In **Saved Sessions**, enter a name (e.g. `codeassist-vps`)  
3. Click **Save**

💡 This way you won’t lose your tunnel settings.

## 5️⃣ CONNECT TO THE VPS

Click **Open** → accept the fingerprint → log in as: `root`
(login: root)

- If you use an SSH key, the connection will start automatically

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

## 1️⃣1️⃣ OPEN CODEASSIST LOCALLY
On your local computer (Windows):
➡️ Open your browser and go to:
`http://localhost:3000`

If everything went well, you’ll see the CodeAssist interface.
Log in using your HuggingFace token, and you’re ready to go 🚀


