# 🔬 GuttyTECH Procedimentos & Ferramentas
> Repositório oficial de **Procedimentos Operacionais Padrão (SOP)** utilizados pela GuttyTECH Performance Solutions para aferição e diagnóstico de hardware em nível de Ring 0. 

A medição de performance não se resume a contadores de FPS (Fraps/Steam). A validação de estabilidade sistêmica exige a medição da cadeia completa de renderização: desde a interrupção de hardware (ISR/DPC) até a resposta do fotodiodo no monitor. Abaixo estão as três frentes do nosso laboratório.

---

## 1. LatencyMon (Auditoria de Kernel e Drivers)
O **LatencyMon** é utilizado para dissecar o comportamento do Windows em Ring 0, rastreando drivers que monopolizam ciclos de CPU e geram *audio popping* ou micro-stuttering.

### Procedimento de Calibragem GuttyTECH:
*   **Isolamento:** A medição deve ocorrer em "Idle" estrito. Navegadores e aplicações de background devem ser encerrados para evitar ruído na amostragem.
*   **Duração:** O teste de rastreio deve ser contínuo por no mínimo **3 minutos**.
*   **Alvos Críticos:** Monitorar especificamente os drivers `ndis.sys` (Rede), `dxgkrnl.sys` (DirectX/GPU) e `nvlddmkm.sys` (NVIDIA).
*   **Métrica de Corte:** O `Current execution time` de qualquer driver não deve ultrapassar a barreira de **1000 µs (microssegundos)**. Ultrapassar este limite acusa falha de agendamento do SO ou conflito IRQ, exigindo a aplicação de `MSI Mode` no componente afetado.

---

## 2. OSLTT (Open Source Latency Testing Tool)
Para validação de **Click-to-Photon** (Tempo de resposta do clique até a tela), abandonamos métricas puramente de software e utilizamos o [OSLTT](https://github.com/pieterjan/OSLTT), uma placa física com fotodiodo acoplada ao monitor.

### Procedimento de Aferição:
*   **Ambiente:** Engine do jogo (ex: CS2 ou Valorant) rodando em tela cheia exclusiva (FSE) para bypass do DWM (Desktop Window Manager) do Windows.
*   **Sensor:** O fotodiodo deve ser posicionado na região de disparo visual (Muzzle Flash) ou animação de ação primária do jogo.
*   **Amostragem:** São realizados 100 disparos (cliques de mouse controlados eletronicamente) para gerar o *Average Input Lag* e o *Standard Deviation* (Desvio Padrão).
*   **Veredito:** Uma máquina calibrada em E-sports deve apresentar latência End-to-End abaixo de **15ms** em monitores de 240Hz+.

---

## 3. HWiNFO64 (Telemetria Térmica e Elétrica)
A estabilidade de um processador após Undervolt ou PBO (Precision Boost Overdrive) não é medida apenas por ausência de tela azul, mas por ausência de *Clock Stretching* (esticamento de clock invisível).

### Procedimento de Stress Test:
*   **Carga:** Utiliza-se o *y-cruncher* ou *OCCT (AVX2)*.
*   **Polling Rate:** No HWiNFO64, o intervalo de pesquisa dos sensores (Sensor Polling Rate) deve ser forçado para **500ms ou 100ms** (contra os 2000ms padrão). Isso impede que picos térmicos rápidos (Spikes) passem despercebidos na leitura.
*   **Análise Pós-Morte:** O log gerado (.CSV) é cruzado com a métrica `Effective Clock`. Se a frequência base (Core Clock) estiver em 4900MHz, mas o `Effective Clock` estiver em 4200MHz durante a carga de 100%, o silício está sofrendo gargalo elétrico ou térmico silencioso, exigindo recalibragem da Curva V/F na BIOS.

---

## 4. CapFrameX / PresentMon (Análise Forense de Frametime)
FPS médio é uma métrica para leigos. O laboratório da GuttyTECH analisa a variância de tempo entre os quadros (Frametime Variance) e o estrangulamento de renderização da API (DirectX/Vulkan) usando **CapFrameX**.

### Procedimento de Captura:
*   **Amostragem Mínima:** Capturas contínuas de 300 segundos (5 minutos) sob estresse real (partidas multijogador, ignorando menus e telas de loading).
*   **Métricas de Corte:** 
    *   `1% Low` e `0.1% Low`: A distância entre a média e os percentis baixos define a gravidade do *stuttering*.
    *   `GPU Busy Time`: Isolamos o tempo de processamento da placa de vídeo para diagnosticar se o gargalo reside na CPU aguardando a API (Render Queue) ou em gargalo térmico. Se o *Frametime* total é alto, mas o *GPU Busy* é baixo, a anomalia reside na lobotomia do Windows (Ring 0) ou nos Timings de RAM.

---

## 5. PingPlotter & Wireshark (Auditoria de Hitreg e Bufferbloat)
Para mitigar a ilusão de que "Ping Baixo é sinônimo de conexão estável", aferimos o roteamento TCP/UDP e a estabilidade do salto de pacotes.

### Procedimento de Inspeção de Rota:
*   **Mapeamento de Hop-by-Hop:** Utilizamos o **PingPlotter** para injetar pacotes (ICMP/UDP) diretamente para os IPs dos servidores da Gamers Club (CS2) e AWS (Rocket League/Fortnite).
*   **Jitter e Packet Loss:** O alvo não é diminuir a distância física (cabos submarinos), mas sim zerar a variação de tempo de chegada dos pacotes (Jitter).
*   **Ajuste TCP/IP Local:** Aplicamos scripts proprietários que desativam algoritmos de coalescência de pacotes (Nagle's Algorithm, RSC) no nível da placa de rede. A validação do envio instantâneo é monitorada através do **Wireshark** para atestar a comunicação sem enfileiramento (Zero Delay Hitreg).

---

*Nota Institucional: Este repositório reflete as práticas internas da GuttyTECH. Se você não é um engenheiro de performance, use os dados como referência acadêmica. Para intervenções diretas na sua máquina e remoção de gargalos descritos acima, acesse [guttytech.com](https://guttytech.com).*
