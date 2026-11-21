
<img width="1678" height="432" alt="image" src="https://github.com/user-attachments/assets/3a5b5c05-8c77-4667-82bd-1a033e7506a5" />

AI that spots grapevine diseases from a leaf photo and maps outbreaks across Georgia’s vineyards.

<p align="center">
  <img src="https://img.shields.io/badge/status-in%20the%20vineyard-B026FF?style=for-the-badge&labelColor=0A0A0F"/>
  <img src="https://img.shields.io/badge/model-EfficientNet--B0-B026FF?style=for-the-badge&labelColor=0A0A0F"/>
  <img src="https://img.shields.io/badge/made%20for-🇬🇪%20Georgia-B026FF?style=for-the-badge&labelColor=0A0A0F"/>
</p>
<p align="center">
  <b>რთველი (rtveli)</b> = the Georgian grape harvest.<br/>
  <b>Rtveli AI</b> = making sure there's still a harvest to celebrate.
</p>
<img src="assets/divider.svg" width="100%"/>
## `> what is this`
 
Every year, **mildew** quietly eats Georgian vineyards.
By the time you can see it clearly, it's often too late.
 
**Rtveli AI** is a tiny vineyard doctor in your pocket.
Snap a grape leaf → the AI tells you what's wrong → you see it spreading on a live map of Georgia.
 
No agronomist on speed dial? No problem.
 
## `> what it can spot`
 
| | disease | the tell |
|:-:|---|---|
| 🟢 | **Healthy** | clean green, no drama |
| 🟣 | **Downy mildew** | yellow oil spots on top, white fuzz underneath |
| 🟣 | **Powdery mildew** | looks like someone dusted flour on it |
| 🟣 | **Black rot** | brown circles with dark edges |
| 🟣 | **Esca** | "tiger stripes" between the veins |
| 🟣 | **Leaf blight** | messy dark brown patches |
| ⚫ | **Not a grape leaf** | the AI says *"I'm not sure"* instead of guessing |
 
>
## `> features`
 
- 📸 **Scan:** photo in, diagnosis out, in about 2 seconds
- 🔥 **Heatmap:** see *exactly* which spots the AI looked at (Grad-CAM)
- 🗺️ **Outbreak map:** anonymous reports light up Kakheti, Imereti, Racha…
- 🍇 **Disease cards:** what it is, why it happened, what to do next
- 🇬🇪 **Bilingual:** English + ქართული
- 🔒 **Private:** photos aren't stored, and locations are blurred to about 1 km
## `> under the hood`
 
```
 leaf photo ──▶ React app ──▶ FastAPI ──▶ EfficientNet (ONNX) ──▶ diagnosis + heatmap
                    ▲                          │
                    └──── Leaflet map ◀── PostgreSQL + PostGIS
```
 
<p>
  <img src="https://img.shields.io/badge/Python-0A0A0F?style=flat-square&logo=python&logoColor=B026FF"/>
  <img src="https://img.shields.io/badge/PyTorch-0A0A0F?style=flat-square&logo=pytorch&logoColor=B026FF"/>
  <img src="https://img.shields.io/badge/ONNX-0A0A0F?style=flat-square&logo=onnx&logoColor=B026FF"/>
  <img src="https://img.shields.io/badge/FastAPI-0A0A0F?style=flat-square&logo=fastapi&logoColor=B026FF"/>
  <img src="https://img.shields.io/badge/PostgreSQL-0A0A0F?style=flat-square&logo=postgresql&logoColor=B026FF"/>
  <img src="https://img.shields.io/badge/React-0A0A0F?style=flat-square&logo=react&logoColor=B026FF"/>
  <img src="https://img.shields.io/badge/Leaflet-0A0A0F?style=flat-square&logo=leaflet&logoColor=B026FF"/>
  <img src="https://img.shields.io/badge/Tailwind-0A0A0F?style=flat-square&logo=tailwindcss&logoColor=B026FF"/>
  <img src="https://img.shields.io/badge/Docker-0A0A0F?style=flat-square&logo=docker&logoColor=B026FF"/>
</p>
**The brain:** EfficientNet-B0, pretrained on ImageNet and fine-tuned on grape leaves
(PlantVillage + field photos). It's trained in two stages, with heavy augmentation, because real farm photos are messy.
 
**The honest part:** we report accuracy on clean lab photos **and** on real vineyard photos.
The second number is the one that matters.
 
## `> run it`
 
```bash
git clone https://github.com/whoiselene/rtveli-ai.git
cd rtveli-ai
docker compose up
```
 
🍇 frontend → `localhost:5173`  ·  ⚡ api docs → `localhost:8000/docs`
 
## `> roadmap`
 
- [x] idea, name, design
- [ ] train the model
- [ ] FastAPI backend
- [ ] React app + heatmap slider
- [ ] outbreak map
- [ ] ქართული version + live demo
- [ ] 🔮 mildew risk forecast from the weather

> [!NOTE]
> Rtveli AI is a helper, not a replacement for an agronomist.
> If the vineyard is in trouble, call a human. 🍷
 
<p align="center">
  <sub>built with 💜 by <a href="https://github.com/whoiselene">Elene Samsiani</a> · tbilisi → budapest</sub>
</p>
 
