# Estufa inteligente na plataforma FIWARE — Grupo `juliaestufa2026`

> Modelo de documentação do repositório do Projeto 2 (CCM520 – Internet das Coisas). Substitua tudo o que está entre `< >` e apague as instruções em itálico. O objetivo é que **outra pessoa consiga reproduzir o projeto** só com este README.

**Integrantes:** `<nome (RA)>`, `<nome (RA)>`
**Link do Wokwi (se usar):** `<link>`
**Vídeo da demonstração (opcional):** `<link>`

## 1. Descrição

_Em um parágrafo: o que a estufa monitora, o que ela aciona e como a plataforma FIWARE entra no sistema._

## 2. Arquitetura

_Um diagrama (imagem ou ASCII) com a ESP32, o broker público, o IoT Agent, o Orion, o Cygnus/MySQL, o Grafana e o consumidor. Indique protocolo e porta em cada ligação._

## 3. Hardware

| Componente | Pino da ESP32 | Observação |
| --- | --- | --- |
| DHT22 | `GPIO4` | temperatura e umidade do ar |
| Umidade do solo | `GPIO34` | ADC, atenuação 11 dB (0 a 4095) |
| LDR | `GPIO35` | ADC, atenuação 11 dB (0 a 4095) |
| Relé | `GPIO26` | saída digital (irrigação) |
| Servomotor | `GPIO27` | PWM 50 Hz (janela, 0° a 180°) |

**Calibração:** _como vocês converteram as leituras do ADC em % de umidade do solo e de luminosidade (valores de referência medidos)._

## 4. Identificadores

| Item | Valor |
| --- | --- |
| API key | `juliaestufa2026` |
| Client id da ponte | `ponte_juliaestufa2026` |
| `fiware-service` / `fiware-servicepath` | `fei` / `/estufa` |
| `device_id` | `estufa001` |
| Entidade | `urn:ngsi-ld:Greenhouse:001` (`Greenhouse`) |
| Client id MQTT da ESP32 | `esp32_juliaestufa2026_estufa001` |

## 5. Modelo de dados

### 5.1 Medidas

| `object_id` (firmware) | Atributo NGSI | Tipo | Unidade / faixa |
| --- | --- | --- | --- |
| `t` | `temperature` | Number | °C |
| `h` | `relativeHumidity` | Number | % |
| `s` | `soilMoisture` | Number | % |
| `l` | `illuminance` | Number | % |
| `r` | `relayState` | Number | 0/1 |
| `a` | `windowAngle` | Number | 0–180 |
| `i` | `samplingInterval` | Number | s |

Exemplo de payload publicado: `{"i": 2, "l": 75.6, "t": 24.0, "a": 0, "h": 40.0, "s": 100, "r": 0}`

### 5.2 Comandos

| Comando | Valores aceitos | Resposta de sucesso | Respostas de erro |
| --- | --- | --- | --- |
| `setRelay` | `0` ou `1` | `OK` | `ERRO: setRelay aceita 0 ou 1`, `ERRO: valor invalido` |
| `setServo` | `0` a `180` | `OK` | `ERRO: setServo aceita 0 a 180`, `ERRO: valor invalido` |
| `setInterval` | `2` a `10` (segundos) | `OK` | `ERRO: setInterval aceita 2 a 10 segundos`, `ERRO: valor invalido` |
| `setSending` | `1` (ativa) ou `0` (desativa) | `OK` | `ERRO: setSending aceita 0 ou 1`, `ERRO: valor invalido` |

### 5.3 Tópicos MQTT

| Sentido | Tópico |
| --- | --- |
| Medidas | `/json/juliaestufa2026/estufa001/attrs` |
| Comandos | `/juliaestufa2026/estufa001/cmd` |
| Resultado | `/json/juliaestufa2026/estufa001/cmdexe` |

### 5.4 Ponte com o broker público

| Padrão | Direção | Por quê |
| --- | --- | --- |
| `/json/juliaestufa2026/estufa001/attrs` | `in` | medidas que a estufa publica no HiveMQ precisam chegar ao IoT Agent local |
| `/json/juliaestufa2026/estufa001/cmdexe` | `in` | resultado (`OK`/`ERRO`) dos comandos executados pela estufa volta ao IoT Agent |
| `/juliaestufa2026/estufa001/cmd` | `out` | comandos que o IoT Agent publica localmente precisam chegar à ESP32 pelo HiveMQ |

## 6. Como reproduzir

_Passo a passo numerado, do `git clone` até o painel funcionando: `mosquitto.conf` (ponte), `docker compose up -d`, teste da ponte, importação da coleção, ordem das requisições, gravação do firmware, criação da fonte de dados e do painel. Informe o que o leitor deve ver ao final de cada passo._

## 7. Subscrições

| Subscrição | Condição | Destino | Atributos e formato | Por que assim |
| --- | --- | --- | --- | --- |
| Histórico | `<...>` | `<...>` | `<...>` | `<...>` |
| Alerta de solo seco | `<...>` | `<...>` | `<...>` | `<...>` |

## 8. Painel

_Captura de tela do painel "Estufa inteligente" e uma linha sobre cada visualização (consulta usada e o que ela mostra). O JSON exportado está em `plataforma/painel-estufa.json`._

## 9. Testes

_Captura do Runner do Postman com a coleção inteira passando (pasta 99 antes) e o arquivo de resultados exportado em `postman/`._

## 10. Decisões de projeto e limitações

_O que vocês escolheram e por quê (calibração, validação dos comandos, tratamento de falhas), o que não funciona bem e o que fariam diferente em um sistema real (pense no broker público)._

## 11. Desafios realizados

_Quais desafios (D.1 a D.4) foram feitos e onde estão no repositório._

## 12. Uso de ferramentas de IA

_Declare, conforme as regras de integridade acadêmica da disciplina, em que partes usaram modelos de linguagem e para quê (por exemplo: explicação de um erro, revisão do texto)._

## 13. Referências

_Material das aulas, documentação do FIWARE e da HiveMQ, e qualquer outra fonte consultada._
