# Open WebUI Troubleshooting Guide

## Understanding the Open WebUI Architecture

The Open WebUI system is designed to streamline interactions between the client (your browser) and the Ollama API. At the heart of this design is a backend reverse proxy, enhancing security and resolving CORS issues.

- **How it Works**: The Open WebUI is designed to interact with the Ollama API through a specific route. When a request is made from the WebUI to Ollama, it is not directly sent to the Ollama API. Initially, the request is sent to the Open WebUI backend via `/ollama` route. From there, the backend is responsible for forwarding the request to the Ollama API. This forwarding is accomplished by using the route specified in the `OLLAMA_BASE_URL` environment variable. Therefore, a request made to `/ollama` in the WebUI is effectively the same as making a request to `OLLAMA_BASE_URL` in the backend. For instance, a request to `/ollama/api/tags` in the WebUI is equivalent to `OLLAMA_BASE_URL/api/tags` in the backend.

- **Security Benefits**: This design prevents direct exposure of the Ollama API to the frontend, safeguarding against potential CORS (Cross-Origin Resource Sharing) issues and unauthorized access. Requiring authentication to access the Ollama API further enhances this security layer.

## Open WebUI: Server Connection Error

If you're experiencing connection issues, it’s often due to the WebUI docker container not being able to reach the Ollama server at 127.0.0.1:11434 (host.docker.internal:11434) inside the container . Use the `--network=host` flag in your docker command to resolve this. Note that the port changes from 3000 to 8080, resulting in the link: `http://localhost:8080`.

**Example Docker Command**:

```bash
docker run -d --network=host -v open-webui:/app/backend/data -e OLLAMA_BASE_URL=http://127.0.0.1:11434 --name open-webui --restart always ghcr.io/open-webui/open-webui:main
```

### Error on Slow Responses for Ollama

Open WebUI has a default timeout of 5 minutes for Ollama to finish generating the response. If needed, this can be adjusted via the environment variable AIOHTTP_CLIENT_TIMEOUT, which sets the timeout in seconds.

### General Connection Errors

**Ensure Ollama Version is Up-to-Date**: Always start by checking that you have the latest version of Ollama. Visit [Ollama's official site](https://ollama.com/) for the latest updates.

**Troubleshooting Steps**:

1. **Verify Ollama URL Format**:
   - When running the Web UI container, ensure the `OLLAMA_BASE_URL` is correctly set. (e.g., `http://192.168.1.1:11434` for different host setups).
   - In the Open WebUI, navigate to "Settings" > "General".
   - Confirm that the Ollama Server URL is correctly set to `[OLLAMA URL]` (e.g., `http://localhost:11434`).

By following these enhanced troubleshooting steps, connection issues should be effectively resolved. For further assistance or queries, feel free to reach out to us on our community Discord.




##### 04-29-2026
Good move—this is where your project becomes maintainable 👍
Let’s turn everything you’ve learned into a **clean, reusable troubleshooting doc** for *Quantum Scribe*.

I’ll structure it so you can drop it straight into your repo as:

```text
docs/TROUBLESHOOTING.md
```

---

# 📘 Quantum Scribe – Troubleshooting Guide

## 🔧 1. Open WebUI “Backend Required” Error

### ❌ Error

```text
Open WebUI Backend Required
You're using an unsupported method (frontend only)
```

### ✅ Cause

Frontend (`npm run dev` on port 5173) is running without backend.

### ✅ Fix

* Run backend:

```bash
cd backend
python -m uvicorn open_webui.main:app --host 0.0.0.0 --port 8080
```

* OR use production build:

```bash
npm run build
```

Then access:

```text
http://<server-ip>:8080
```

---

## 🧱 2. Backend Shows “Not Found” on Port 8080

### ❌ Error

```json
{"detail": "Not Found"}
```

### ✅ Cause

Frontend build not present.

### ✅ Fix

```bash
cd ~/quantum-scribe
export NODE_OPTIONS="--max-old-space-size=8192"
npm install
npm run build
```

Restart backend after build.

---

## 💥 3. Node Heap Memory Error (Build Fails)

### ❌ Error

```text
JavaScript heap out of memory
```

### ✅ Fix

```bash
export NODE_OPTIONS="--max-old-space-size=8192"
npm run build
```

Optional clean rebuild:

```bash
rm -rf node_modules .svelte-kit build
npm install --force
npm run build
```

---

## 🎨 4. Favicon Not Updating

### ❌ Problem

Browser still shows old icon.

### ✅ Fix

1. Replace source file:

```bash
static/favicon.png
```

2. Check:

```html
src/app.html
```

3. Rebuild:

```bash
npm run build
```

4. Clear browser cache / use incognito

---

## ⚠️ 5. GPU Not Used (AMD)

### ❌ Symptoms

* CPU high usage
* VRAM very low
* radeontop shows graphics activity only

### ✅ Cause

ROCm not installed or unsupported GPU

### ✅ Fix

* Install ROCm (if supported)
* Verify:

```bash
rocminfo
```

---

## 🔥 6. GPU Active but Low VRAM Usage (~30%)

### ❌ Misconception

GPU not fully used

### ✅ Explanation

VRAM usage depends on model size, not capacity.

Example:

```text
llama3 8B ≈ 5–8GB VRAM
```

### ✅ Status

✔ Normal behavior

---

## 🧠 7. CPU Usage Only ~20–35%

### ❌ Concern

GPU not working

### ✅ Explanation

* GPU handles inference
* CPU handles tokenization, I/O, scheduling

### ✅ Status

✔ Healthy mixed workload

---

## 🚀 8. Performance Drops with Multiple Users

### ❌ Symptoms

* Slow responses
* Delayed generation

### ✅ Cause

Ollama concurrency limits

### ✅ Fix

Tune:

```bash
export OLLAMA_NUM_PARALLEL=2
export OLLAMA_MAX_LOADED_MODELS=1
```

Test scaling gradually:

```bash
OLLAMA_NUM_PARALLEL=4
```

---

## 👥 9. Handling Multiple Users

### Expected capacity (16GB GPU)

| Active Requests | Performance   |
| --------------- | ------------- |
| 1–3             | Fast          |
| 4–8             | Moderate      |
| 10+             | Slow / queued |

### Recommendation

* Use smaller model (llama3 8B Q4)
* Avoid heavy concurrent prompts

---

## 🔌 10. Ollama Not Showing Models in WebUI

### ❌ Problem

No models listed

### ✅ Fix

Check:

```bash
curl http://localhost:11434/api/tags
```

If empty:

```bash
ollama pull llama3
```

---

## 🌐 11. Cannot Access from Other Devices

### ❌ Problem

Works locally, not on network

### ✅ Fix

Run backend with:

```bash
--host 0.0.0.0
```

Access via:

```text
http://<server-ip>:8080
```

---

## 🧼 12. UI Issues / Old Version Still Showing

### Fix

* Hard refresh:

```
Ctrl + Shift + R
```

* Or open incognito
* Or clear cache

---

# 🧭 Recommended Monitoring Tools

```bash
htop        # CPU usage
radeontop   # AMD GPU usage
ollama ps   # loaded models
```

---

# 🚀 Future Improvements

* Reverse proxy (NGINX + HTTPS)
* GPU optimization (ROCm tuning)
* Multi-instance Ollama for scaling
* Load balancing

---

# 👍 Maintainer Notes

* Always rebuild frontend after changes
* Never edit `/build` manually
* Keep `backend/data/` for persistence
* Sync upstream regularly if using fork

---

## If you want next level

I can help you:

* turn this into **auto-documented README**
* add **health check dashboard**
* build **admin monitoring panel for Quantum Scribe**
* or create **multi-user scaling architecture**

Just say 👍
