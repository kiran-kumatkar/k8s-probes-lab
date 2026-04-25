\# 🚀 Kubernetes Probes Deep Dive



\## 📌 Overview



This project demonstrates how health checks work in \*\*Kubernetes\*\* using hands-on labs on a local cluster created with \*\*kind\*\*.



This lab focuses purely on \*\*understanding probe behavior, failure scenarios, and debugging techniques\*\* using lightweight containers.



\---



\## 🎯 Objectives



\* Understand \*\*Liveness, Readiness, and Startup Probes\*\*

\* Observe how Kubernetes reacts to failures

\* Simulate real-world misconfigurations

\* Learn debugging using `kubectl describe` and events



\---



\## 🧠 What are Probes?



Kubernetes uses probes to check container health:



\### 🔹 Liveness Probe



\* Detects if container is \*\*dead or stuck\*\*

\* If it fails → container is \*\*restarted\*\*



\### 🔹 Readiness Probe



\* Detects if container is \*\*ready to serve traffic\*\*

\* If it fails → pod is \*\*removed from service endpoints\*\*



\### 🔹 Startup Probe



\* Used for \*\*slow-starting applications\*\*

\* Prevents liveness probe from killing container too early



\---



\## ⚙️ Setup Instructions



\### 1️⃣ Create Cluster



```bash

kind create cluster

kubectl cluster-info

```



\---



\## 🧪 Hands-on Labs



\---



\### 🔬 1. Liveness Probe (Self-Healing Behavior)



Uses \*\*busybox\*\* to simulate failure.



✔ Container creates a file → healthy

❌ File removed after 30s → probe fails → restart



\#### Key Learning



\* Liveness probe \*\*restarts container\*\*

\* Does NOT fix application logic



\---



\### 🔬 2. Readiness Probe (Traffic Control)



Uses \*\*nginx\*\* with incorrect path.



✔ Pod is Running

❌ But not Ready (`0/1`)



\#### Key Learning



\* Pod can be running but \*\*not serve traffic\*\*

\* Readiness controls \*\*service routing\*\*



\---



\### 🔬 3. Startup Probe (Slow App Handling)



Simulated using delayed file creation.



✔ Prevents premature restarts

✔ Gives container time to initialize



\#### Key Learning



\* Startup probe protects \*\*slow applications\*\*



\---



\### 🔬 4. Failure Scenarios (Real-World Cases)



Tested multiple misconfigurations:



\* ❌ Wrong path (`/wrong`)

\* ❌ Wrong port

\* ❌ Aggressive timing (low delay/timeout)



\#### Key Learning



\* Misconfigured probes can:



&#x20; \* Cause restart loops

&#x20; \* Keep pods permanently unready



\---



\## 🔁 Understanding CrashLoopBackOff



Observed behavior:



```bash

kubectl get pods -w

```



\### Key Observations



\* Container repeatedly restarts

\* Eventually enters `CrashLoopBackOff`



\### What it Means



\* Kubernetes detected \*\*repeated failures\*\*

\* Applies \*\*exponential backoff delay\*\*

\* Still retries container after delay



\---



\## 🔍 Debugging Techniques



Used:



```bash

kubectl describe pod <pod-name>

kubectl get events

```



\### What to look for:



\* `Liveness probe failed`

\* `Readiness probe failed`

\* `Back-off restarting failed container`



\---



\## 📸 Screenshots



All command outputs and observations are captured in the \*\*\[screenshots](screenshots/)\*\* folder.



\---



\## 📂 Project Structure



```

k8s-probes-lab/

│

├── liveness-demo.yml

├── readiness-demo.yml

├── startup-demo.yml

├── screenshots/

└── README.md

```



\---



\## 💡 Key Learnings



\* Liveness probe ensures \*\*container restarts\*\*

\* Readiness probe controls \*\*traffic flow\*\*

\* Startup probe prevents \*\*false failures during boot\*\*

\* CrashLoopBackOff = \*\*retry with delay, not permanent failure\*\*

\* Probes do NOT fix applications — they only manage lifecycle



\---



\## 🎯 Interview Takeaways



\* Pod can be \*\*Running but Not Ready\*\*

\* Liveness failure leads to \*\*restart loops\*\*

\* CrashLoopBackOff uses \*\*exponential backoff\*\*

\* Misconfigured probes are a \*\*common production issue\*\*



\---



\## 🚀 Conclusion



This project focuses on \*\*understanding Kubernetes behavior under failure conditions\*\*, which is critical for real-world troubleshooting and production environments.



\---



\## ⭐ Future Improvements



\* Add Service + observe readiness impact

\* Integrate with Ingress

\* Visualize probe failures using monitoring tools



\---



\## 🙌 Author



Kiran Kumatkar



