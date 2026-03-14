<div align="center">

# 🐾 PIXEL ANIMAL MATCH 🐾

### *Retro-pixel tarzında hafıza kartı eşleştirme oyunu*

[![Play Now](https://img.shields.io/badge/🎮_OYNA-HEMEN-FF4365?style=for-the-badge&logoColor=white)](https://omeryasirkucuk.github.io/pixel-animal-match/)
[![License](https://img.shields.io/badge/License-MIT-16c79a?style=for-the-badge)](LICENSE)
[![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)]()
[![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white)]()
[![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)]()

<br>

<img src="https://img.shields.io/badge/-%F0%9F%90%B6%20%F0%9F%90%B1%20%F0%9F%90%AD%20%F0%9F%90%B9%20%F0%9F%90%B0%20%F0%9F%A6%8A%20%F0%9F%90%BB%20%F0%9F%90%BC-1a1a2e?style=for-the-badge" />

---

*32 farklı hayvan • 3 zorluk seviyesi • Sıfır bağımlılık • Tek dosya*

</div>

<br>

## 🎯 Nedir?

**Pixel Animal Match**, tamamen vanilla HTML/CSS/JS ile yazılmış, retro pixel-art estetiğine sahip bir **hafıza kartı eşleştirme** oyunudur. Kartları çevir, hayvan çiftlerini bul, en az hamleyle bitir!

<div align="center">

```
┌─────┐ ┌─────┐ ┌─────┐ ┌─────┐
│  ?  │ │ 🐶  │ │  ?  │ │ 🦊  │
└─────┘ └─────┘ └─────┘ └─────┘
┌─────┐ ┌─────┐ ┌─────┐ ┌─────┐
│ 🐱  │ │  ?  │ │ 🐱  │ │  ?  │
└─────┘ └─────┘ └─────┘ └─────┘
```

</div>

## ✨ Özellikler

| Özellik | Detay |
|---------|-------|
| 🎮 **3 Zorluk Seviyesi** | Kolay (4×4), Zor (6×6), Hardcore (8×8) |
| 🐾 **32 Hayvan** | Köpekten flamingoya, köpekbalığından kelebeğe |
| 🎵 **Retro Ses Efektleri** | Web Audio API ile pixel-tarzı sesler |
| 🔥 **Combo Sistemi** | Üst üste eşleştirmelerle combo yap! |
| 🏆 **Skor Tablosu** | localStorage ile kalıcı en iyi 5 skor |
| 🎊 **Confetti Animasyonu** | Kazandığında kutlama efekti |
| 📱 **Responsive** | Mobil, tablet ve masaüstü uyumlu |
| ⚡ **Sıfır Bağımlılık** | Framework yok, build yok, tek `index.html` |

## 🚀 Hemen Oyna

### 🌐 Online
👉 **[omeryasirkucuk.github.io/pixel-animal-match](https://omeryasirkucuk.github.io/pixel-animal-match/)**

### 💻 Lokalde
```bash
git clone https://github.com/omeryasirkucuk/pixel-animal-match.git
cd pixel-animal-match
open index.html   # macOS
# veya
xdg-open index.html   # Linux
```

## 🎮 Nasıl Oynanır?

```
1️⃣  Zorluk seviyesi seç
2️⃣  Bir karta tıkla → kart döner
3️⃣  İkinci kartı tıkla → eşleşme kontrolü
4️⃣  Eşleşirse → kartlar açık kalır ✅
5️⃣  Eşleşmezse → kartlar geri döner ❌
6️⃣  Tüm çiftleri bul → kazandın! 🎊
```

> 💡 **İpucu:** Üst üste eşleştirme yaparak **COMBO** kazan ve skorunu düşür!

## 🐾 Hayvan Kadrosu

<div align="center">

| | | | | | | | |
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| 🐶 | 🐱 | 🐭 | 🐹 | 🐰 | 🦊 | 🐻 | 🐼 |
| Köpek | Kedi | Fare | Hamster | Tavşan | Tilki | Ayı | Panda |
| 🐨 | 🐯 | 🦁 | 🐮 | 🐷 | 🐸 | 🐵 | 🐔 |
| Koala | Kaplan | Aslan | İnek | Domuz | Kurbağa | Maymun | Tavuk |
| 🦄 | 🐴 | 🦋 | 🐢 | 🐍 | 🦎 | 🐙 | 🦀 |
| Unicorn | At | Kelebek | Kaplumbağa | Yılan | Kertenkele | Ahtapot | Yengeç |
| 🐬 | 🦈 | 🐘 | 🦒 | 🦓 | 🐊 | 🦜 | 🦩 |
| Yunus | Köpekbalığı | Fil | Zürafa | Zebra | Timsah | Papağan | Flamingo |

</div>

## 🏗️ Teknik Detaylar

```
📦 pixel-animal-match
├── index.html        ← Tek dosya, tüm oyun burada
├── README.md
└── docs/
```

- **Render:** Pure DOM manipulation, no virtual DOM
- **Animasyonlar:** CSS `@keyframes` + `transform`
- **Ses:** Web Audio API oscillator sentezi
- **Veri:** `localStorage` ile skor persistance
- **Font:** Google Fonts — Press Start 2P

## 📊 Skor Hesaplama

```
SKOR = Hamle Sayısı + Geçen Süre (saniye)
```

> Düşük skor = daha iyi! En az hamleyle, en hızlı şekilde bitirmeye çalış.

---

<div align="center">

**Pixel Animal Match** ile yapıldı 💚

[⬆ Başa Dön](#-pixel-animal-match-)

</div>
