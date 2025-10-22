# 🌿 Preliminary Design Review (PDR)
## Project: **PlantSense – Edge AI Plant Identifier (React PWA)**  

### **1. Project Overview**
**PlantSense** is a **Progressive Web Application (PWA)** built with **React (Vite)** that identifies plants from images using an **on-device AI model (Edge AI)**.  

It runs fully in the browser (desktop or mobile) and can work **offline** after initial installation.  
When online, the app can **update its database** or **search the internet** for plants not found in its local collection — but **only after user consent**.

The app can analyze both:
- 🌸 **Live camera captures**, and  
- 📸 **Uploaded images** from the device gallery or file system.

---

### **2. Objectives**
1. Identify plants instantly from a captured or uploaded image.  
2. Display plant details including:
   - Common and botanical names  
   - Description and physical features  
   - Medicinal or economic importance  
   - Best growing conditions (climate, soil, sunlight, etc.)  
   - Geographic distribution or “where to find” information  
3. Operate offline after first use (PWA caching).  
4. Allow optional online database update or web search when plant is unknown.  
5. Prompt the user before performing any online operation.

---

### **3. System Architecture**
**Architecture Type:** Hybrid Edge + Cloud  

| Layer | Component | Description |
|-------|------------|-------------|
| **Frontend (PWA)** | React (Vite) + TensorFlow.js | User interface, camera access, and on-device inference. |
| **Offline Storage** | IndexedDB / LocalStorage | Stores plant data and cached results for offline mode. |
| **Edge AI Model** | TensorFlow.js (MobileNetV3 or EfficientNet-lite) | Performs image classification locally in the browser. |
| **Service Worker** | Workbox + Vite Plugin PWA | Handles caching, offline use, and background updates. |
| **Cloud Backend (Optional)** | Node.js + Express API | Provides online data enrichment, updates, or new species. |
| **Database (Server)** | PostgreSQL / MongoDB | Stores plant metadata, model updates, and reference data. |

---

### **4. Functional Flow**
1. **User Action:** Opens the app → chooses *Capture* or *Upload Image*.  
2. **Inference (Edge AI):** TensorFlow.js model predicts possible species.  
3. **Local Response:**  
   - If **confidence ≥ threshold**, display details from local data store.  
   - If **confidence < threshold**, prompt user:  
     > “I couldn’t identify this plant with high confidence. Would you like me to search online?”  
4. **Online Mode (with consent):**  
   - Sends minimal data (image fingerprint or label) to backend/cloud API.  
   - Retrieves information from public APIs (GBIF, Wikipedia, POWO, etc.).  
   - Updates local cache and enriches plant database.  
5. **Offline Cache:** Newly added information is stored locally for future offline use.

---

### **5. Key Components**
| Module | Description |
|---------|--------------|
| **Camera Capture** | Uses `getUserMedia()` to access webcam; captures still images. |
| **Image Upload Handler** | Allows users to upload existing images for identification. |
| **AI Model Loader** | Loads and initializes the pre-trained TensorFlow.js model. |
| **Inference Engine** | Processes input images → predicts plant species + confidence score. |
| **Local Database Manager** | Manages IndexedDB records for offline plant data. |
| **Sync Manager** | Checks for updates and syncs new data when online. |
| **UI Components** | Responsive plant cards showing name, description, and features. |
| **User Consent Dialog** | Prompts for permission before performing online lookups. |

---

### **6. Technologies**
| Category | Choice |
|-----------|--------|
| **Frontend Framework** | React 18 + Vite |
| **Edge AI Library** | TensorFlow.js |
| **Styling** | Tailwind CSS / ShadCN UI |
| **PWA Tools** | Vite Plugin PWA, Workbox |
| **Offline Storage** | IndexedDB (via Dexie.js) |
| **Backend (Optional)** | Node.js + Express + PostgreSQL |
| **External APIs** | GBIF API, Wikipedia API, Plants of the World Online (POWO) |

---

### **7. Minimum Viable Product (MVP)**
✅ Upload or capture plant image  
✅ On-device identification via TensorFlow.js  
✅ Show common name + brief description from local database  
✅ Full offline functionality via PWA caching  
✅ User consent before any online search  
✅ Local caching of new plants after online lookup  

---

### **8. Future Enhancements**
- 🌍 Map visualization of plant distribution  
- 💊 Expanded database of medicinal/economic uses  
- 🧠 Continuous learning (federated model updates)  
- 📷 Augmented Reality mode for plant part labeling  
- 👥 User-submitted plant photos for community data expansion  

---

### **9. Risks & Mitigations**
| Risk | Mitigation |
|------|-------------|
| Large AI model size → slow loading | Use quantized MobileNetV3-Lite model |
| Browser compatibility | Test across Chrome, Edge, Firefox, Safari |
| Unavailable APIs during fetch | Cache last successful responses |
| Misidentification | Show top 3 results + confidence scores |

---

### **10. Image Access (Camera + Upload)**
PlantSense can process:
1. **Live camera captures** — via the browser’s `navigator.mediaDevices.getUserMedia()` API.  
2. **Uploaded images** — users can upload `.jpg`, `.jpeg`, or `.png` files through a file input field.  

The uploaded or captured image is passed to the TensorFlow.js model for inference without being sent to any external server (ensuring privacy).  

---

### **11. Next Steps**
1. ✅ Initialize Vite + React + PWA scaffolding.  
2. ✅ Integrate TensorFlow.js and load a sample MobileNet model.  
3. ✅ Implement camera and image upload components.  
4. ✅ Add inference logic and display predicted plant details.  
5. ⚙️ Connect IndexedDB for offline metadata storage.  
6. 🌐 Build an optional backend/API for database enrichment.  
7. 🧠 Package and optimize PWA for offline-first operation.  

---

### **12. References**
- [TensorFlow.js Models](https://www.tensorflow.org/js/models)  
- [GBIF API Documentation](https://www.gbif.org/developer/summary)  
- [Wikipedia REST API](https://www.mediawiki.org/wiki/API:REST_API)  
- [Plants of the World Online](https://powo.science.kew.org/)  
- [Workbox PWA Docs](https://developer.chrome.com/docs/workbox)

---

### **13. System Architecture Diagram**

```mermaid
flowchart TD

subgraph User["User"]
  A1["Capture Image"] --> A2["Or Upload Image"]
end

subgraph PWA["PlantSense (React + Vite PWA)"]
  B1["TensorFlow.js Model (Edge AI)"]
  B2["Local Plant Database (IndexedDB)"]
  B3["Inference Engine"]
  B4["Service Worker (Offline Cache)"]
  B5["Consent Dialog"]
end

subgraph Cloud["Cloud Services (Optional)"]
  C1["Node.js API Gateway"]
  C2["Plant Metadata DB (PostgreSQL / MongoDB)"]
  C3["External APIs (GBIF / POWO / Wikipedia)"]
end

%% Flow connections
User -->|Image Input| PWA
A2 -->|Captured/Uploaded Image| B3
B3 -->|Run Inference| B1
B1 -->|Predicted Species + Confidence| B2
B2 -->|Local Lookup| B3

%% Decision Branch
B3 -->|Confidence >= Threshold| D1["Show Plant Info (Offline Mode)"]
B3 -->|Confidence < Threshold| B5
B5 -->|User Grants Permission| C1
B5 -->|User Declines| D2["Display 'Not Found' Message"]

%% Cloud interactions
C1 -->|Query / Fetch Data| C2
C1 -->|Fetch Enrichment Data| C3
C3 -->|Return Description / Metadata| C1
C2 -->|Send Update Manifest| B4
C1 -->|Return Enriched Plant Info| B2

%% Offline sync
B4 -->|Sync Cached Data| B2
B2 -->|Updated Local Records| D3["Display Updated Plant Info"]

style User fill:#e6ffe6,stroke:#228B22,stroke-width:2px
style PWA fill:#f5f5ff,stroke:#4169E1,stroke-width:2px
style Cloud fill:#fff8dc,stroke:#DAA520,stroke-width:2px



© 2025 PlantSense – Edge AI for Sustainable Awareness 🌿
