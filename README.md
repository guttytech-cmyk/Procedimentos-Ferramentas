<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0A0A0A&height=120&section=header&text=PROCEDIMENTOS%20%26%20FERRAMENTAS&fontSize=32&fontColor=E50A0A&animation=fadeIn" alt="Procedimentos" />

[![GuttyTECH](https://img.shields.io/badge/Lab-GuttyTECH_Ring--0-E50A0A?style=for-the-badge)](https://guttytech.com)
[![SOP](https://img.shields.io/badge/Tipo-SOP_Oficial-121212?style=for-the-badge)](https://github.com/guttytech-cmyk/Procedimentos-Ferramentas)
[![Contato](https://img.shields.io/badge/Email-admin@guttytech.com-0078D4?style=for-the-badge)](mailto:admin@guttytech.com)

**Procedimentos Operacionais Padrao para afericao e diagnostico em Ring 0**

[Website](https://guttytech.com) · [Commander](https://github.com/guttytech-cmyk/Commander)

</div>

---

> Performance real nao se resume a FPS medio. Validamos ISR/DPC, click-to-photon, termica, frametime variance e rota de rede — da interrupcao de hardware ao fotodiodo no monitor.

---

## Indice de laboratorio

| # | Ferramenta | Foco | Metrica de corte |
|:-:|------------|------|------------------|
| 1 | **LatencyMon** | Drivers Ring 0 | Execution time < **1000 us** |
| 2 | **OSLTT** | Click-to-photon | E2E < **15ms** @ 240Hz+ |
| 3 | **HWiNFO64** | Termica / elétrica | Effective Clock vs Core Clock |
| 4 | **CapFrameX** | Frametime forensics | 1% Low, 0.1% Low, GPU Busy |
| 5 | **PingPlotter** | Rota / jitter | Jitter e packet loss por hop |

---

## 1. LatencyMon — Kernel e drivers

| Passo | Procedimento |
|-------|--------------|
| Isolamento | Idle estrito — sem browser/background |
| Duracao | Minimo **3 minutos** continuos |
| Alvos | `ndis.sys`, `dxgkrnl.sys`, `nvlddmkm.sys` |
| Falha | Execution time > **1000 us** → aplicar MSI Mode |

---

## 2. OSLTT — Click-to-photon

| Passo | Procedimento |
|-------|--------------|
| Ambiente | FSE (fullscreen exclusive) para bypass DWM |
| Sensor | Fotodiodo no muzzle flash / acao primaria |
| Amostra | **100** disparos controlados |
| Veredito | Media E2E < **15ms** em 240Hz+ |

---

## 3. HWiNFO64 — Stress termico

| Passo | Procedimento |
|-------|--------------|
| Carga | y-cruncher ou OCCT AVX2 |
| Polling | **100–500ms** (nao 2000ms padrao) |
| Analise | Cruzar Core Clock vs **Effective Clock** no CSV |

---

## 4. CapFrameX / PresentMon — Frametime

| Passo | Procedimento |
|-------|--------------|
| Captura | **300s** em partida real (nao menu) |
| Metricas | 1% Low, 0.1% Low, GPU Busy Time |
| Diagnostico | Frametime alto + GPU Busy baixo = gargalo Ring 0 / RAM |

---

## 5. PingPlotter e Wireshark — Rede

| Passo | Procedimento |
|-------|--------------|
| Rota | Hop-by-hop para IPs de servidor alvo |
| Foco | Zerar **jitter**, nao so ping medio |
| TCP/IP | Nagle/RSC off — validar com Wireshark |

---

<div align="center">

*Praticas internas GuttyTECH. Para intervencao na sua maquina, acesse*

[![guttytech.com](https://img.shields.io/badge/Contratar-guttytech.com-E50A0A?style=for-the-badge)](https://guttytech.com)

<img src="https://capsule-render.vercel.app/api?type=waving&color=E50A0A&height=70&section=footer&fontSize=14&fontColor=0A0A0A" alt="footer" />

</div>
