# Rastreamento de Transporte Inteligente — Itapetininga/SP

> Sistema de rastreamento em tempo real de ônibus urbanos da cidade de Itapetininga, com visualização via plataforma web acessível à população.

---

## Sobre o Projeto

O **Rastreamento de Transporte Inteligente** é uma solução de mobilidade urbana desenvolvida para os cidadãos de Itapetininga/SP. O projeto instala dispositivos **Raspberry Pi** nos ônibus da cidade, equipados com chips de conectividade, que transmitem a localização dos veículos em tempo real para uma plataforma web.

Com isso, qualquer pessoa que estiver em um ponto de ônibus poderá acessar o site e ver **exatamente a quantos quilômetros o ônibus está do seu ponto**, sem precisar depender de horários fixos ou esperar sem informação.

---

## Objetivo

Reduzir a incerteza e o tempo de espera nos pontos de ônibus de Itapetininga, tornando o transporte público mais transparente, eficiente e acessível para toda a população.

---

## Como Funciona

```
┌─────────────────────┐       GPS + Chip        ┌──────────────────────┐
│   Ônibus Urbano     │ ──────────────────────► │   Servidor / Nuvem   │
│  (Raspberry Pi +    │                         │  (Processamento de   │
│   Chip de Dados)    │                         │      Localização)    │
└─────────────────────┘                         └──────────┬───────────┘
                                                           │
                                                    API em tempo real
                                                           │
                                                ┌──────────▼───────────┐
                                                │     Site / App Web   │
                                                │  "O ônibus está a    │
                                                │   2,3 km do ponto"   │
                                                └──────────────────────┘
```

1. O **Raspberry Pi** instalado em cada ônibus coleta a localização via GPS continuamente.
2. Os dados são transmitidos via **chip de dados móvel** para o servidor em nuvem.
3. O **servidor** processa as coordenadas e calcula a distância até cada ponto de ônibus.
4. O **site** exibe, em tempo real, a distância e o tempo estimado de chegada do ônibus ao ponto consultado.

---

## Tecnologias Utilizadas

### Hardware
| Componente | Descrição |
|---|---|
| Raspberry Pi | Computador embarcado instalado em cada ônibus |
| Módulo GPS | Coleta de coordenadas geográficas em tempo real |
| Chip de Dados (SIM) | Transmissão dos dados via rede móvel (4G/LTE) |

### Software
| Camada | Tecnologias |
|---|---|
| Firmware (Raspberry Pi) | Python, gpsd, scripts de transmissão |
| Backend / API | A definir (Node.js / FastAPI / etc.) |
| Banco de Dados | A definir (PostgreSQL / Firebase / etc.) |
| Frontend Web | A definir (React / HTML+JS) |
| Infraestrutura | A definir (AWS / GCP / VPS) |

---

## Funcionalidades Previstas

- [x] Definição da arquitetura do sistema
- [ ] Configuração do Raspberry Pi com módulo GPS
- [ ] Transmissão de localização via chip de dados
- [ ] API de backend para receber e processar coordenadas
- [ ] Cálculo de distância entre ônibus e pontos cadastrados
- [ ] Site público com visualização em tempo real
- [ ] Cadastro de todos os pontos de ônibus de Itapetininga
- [ ] Cadastro de todas as linhas de ônibus da cidade
- [ ] Estimativa de tempo de chegada (ETA)
- [ ] Versão mobile-friendly do site

---

## Como Executar (em desenvolvimento)

> O projeto ainda está em fase inicial. As instruções de instalação serão adicionadas conforme o desenvolvimento avança.

---

## Estrutura do Repositório

```
rastreamento-de-transporte-inteligente/
├── hardware/          # Scripts e configurações do Raspberry Pi
├── backend/           # API e servidor
├── frontend/          # Site público de rastreamento
├── docs/              # Documentação técnica
└── README.md
```

---
