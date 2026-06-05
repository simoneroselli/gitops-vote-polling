# Vote Polling App
## 🧩 Application Components

The playground replicates the classic multi-tier Voting Application, broken down by domain boundaries:

### 1. `vote-frontend` (Python Web App)
* **Role:** The client-facing ingestion tier where users cast their votes.
* **Behavior:** Highly sensitive to concurrent connection load. Built using a synchronous processing paradigm, meaning heavy HTTP traffic bursts cause fast CPU utilization spikes due to thread context-switching.

### 2. `redis` (In-Memory Buffer)
* **Role:** The asynchronous queueing layer separating ingestion from database processing.
* **Behavior:** Extremely lightweight and I/O optimized. Acts as a shock absorber, holding raw voting data strings securely in memory without burning significant compute cycles.

### 3. `vote-worker` (.NET Background Processor)
* **Role:** The background worker engine.
* **Behavior:** Continually polls Redis for new items in a single-threaded loop, batches or processes the raw data, and writes the finalized state down to the persistent database.

### 4. `postgres` / `result-frontend` (Data & Presentation)
* **Role:** Persistent storage and the live analytics dashboard displaying voting tallies.

---