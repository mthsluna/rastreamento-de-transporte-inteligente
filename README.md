# Rastreamento de transporte inteligente

Sistema de rastreamento em tempo real de ônibus urbanos de Itapetininga/SP. Raspberry Pi
instalados nos ônibus transmitem localização via chip de dados para um servidor central.
Um site público consome essa API e exibe, para qualquer pessoa em um ponto de ônibus,
a quantos quilômetros o veículo está.

## Como funciona

```
Raspberry Pi (ônibus)
  └── GPS coleta coordenadas
  └── Chip de dados envia para o servidor

Servidor
  └── Recebe e persiste as coordenadas
  └── Calcula distância até cada ponto cadastrado
  └── Expõe API REST/WebSocket

Site público
  └── Usuário seleciona o ponto
  └── Vê distância e ETA de cada linha em tempo real
```

## Estrutura do repositório

```
rastreamento-de-transporte-inteligente/
  hardware/
    gps_tracker.py          # script principal do Raspberry Pi
    sender.py               # transmissão via chip de dados para o servidor
    requirements.txt        # dependências Python do dispositivo
    install.sh              # setup inicial do Raspberry Pi
  backend/
    server.py               # servidor principal (recebe coords, expõe API)
    routes/
      buses.py              # endpoints de localização dos ônibus
      stops.py              # endpoints dos pontos cadastrados
    models/
      bus.py
      stop.py
    db.py                   # conexão com banco de dados
    requirements.txt
  frontend/
    index.html              # página principal
    app.js                  # lógica de consumo da API e atualização em tempo real
    style.css
  docs/
    arquitetura.md          # decisões de design e diagrama detalhado
    pontos-itapetininga.md  # lista dos pontos cadastrados
  README.md
```

## Hardware

Cada ônibus carrega:

- Raspberry Pi (modelo a definir — Zero 2 W ou 3B+)
- Módulo GPS (ex: u-blox NEO-6M ou similar via UART/USB)
- Chip de dados SIM com plano de dados (operadora a definir)

O script `hardware/gps_tracker.py` lê as coordenadas via `gpsd` e as envia
periodicamente para o servidor via HTTP POST. Intervalo de envio configurável
via variável de ambiente `SEND_INTERVAL_SECONDS` (padrão: `5`).

## Configuração do Raspberry Pi

```sh
# No Raspberry Pi, após clonar o repositório:
cd hardware
bash install.sh

# install.sh instala gpsd, as dependências Python e registra o serviço
# como systemd unit para rodar automaticamente no boot.
```

Variáveis de ambiente necessárias no dispositivo:

```sh
SERVER_URL=https://api.seuservidor.com.br/location
BUS_ID=onibus-001
SEND_INTERVAL_SECONDS=5
```

Crie um arquivo `.env` em `hardware/` ou exporte as variáveis no ambiente do serviço.

## Backend

```sh
cd backend
pip install -r requirements.txt

# Configurar variáveis de ambiente:
DATABASE_URL=postgresql://user:pass@localhost/rastreamento
PORT=8000

python server.py
```

### Endpoints principais

```
POST /location
  Body: { "bus_id": "onibus-001", "lat": -23.59, "lon": -48.05, "timestamp": "..." }
  Recebe coordenadas dos Raspberry Pi.

GET /buses
  Retorna posição atual de todos os ônibus ativos.

GET /stops
  Retorna lista de pontos cadastrados com coordenadas.

GET /stops/:stop_id/distances
  Retorna distância e ETA de cada ônibus até o ponto informado.
```

## Frontend

O site é estático. Basta servir a pasta `frontend/` com qualquer servidor HTTP.

```sh
cd frontend
npx serve .
# ou
python3 -m http.server 3000
```

O usuário seleciona o ponto de ônibus no site e vê em tempo real a distância
de cada linha. Atualizações via polling à API (intervalo configurável em `app.js`).

## Pontos de ônibus

Os pontos cadastrados ficam em `docs/pontos-itapetininga.md` e são importados
no banco via script:

```sh
cd backend
python scripts/import_stops.py ../docs/pontos-itapetininga.md
```

## Status do projeto

- [x] Definição da arquitetura
- [ ] Script GPS no Raspberry Pi
- [ ] Transmissão via chip de dados
- [ ] Servidor recebendo coordenadas
- [ ] API de distância por ponto
- [ ] Site público
- [ ] Cadastro completo dos pontos de Itapetininga
- [ ] Deploy em produção

## Documentação

- [Arquitetura e decisões de design](docs/arquitetura.md)
- [Pontos de ônibus cadastrados](docs/pontos-itapetininga.md)

## Contexto

Itapetininga tem mais de 160 mil habitantes e o transporte público local carece
de informação em tempo real nos pontos. Este projeto usa hardware acessível
(Raspberry Pi) e conectividade móvel para resolver um problema real do dia a dia
da população.

O repositório não distribui credenciais, chaves de API, dados de usuários
ou configurações de produção. Esses valores devem ser fornecidos via variáveis
de ambiente em cada ambiente de deploy.
