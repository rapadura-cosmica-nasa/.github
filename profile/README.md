# 🚀 Rapadura Côsmica — NASA Space Apps Challenge 2026

> **Local Event:** Campina Grande, Paraíba, Brazil


> **Challenge:** [Create Health Monitoring Software for Astronauts on Space Missions](https://www.spaceappschallenge.org/2026/challenges/health-monitoring-software/)

---

## 🌌 Sobre o Projeto

Somos a **Rapadura Côsmica**, uma equipe de estudantes de Ciência da Computação, Engenharia da Computação e Análise e Desenvolvimento de Sistemas de Campina Grande (PB). Assim como a rapadura — resistente, energética e essencial no sertão —, construímos uma arquitetura resiliente para apoiar astronautas em missões de longa duração.

Em missões espaciais profundas, a comunicação com a Terra sofre atrasos significativos, exigindo que a tripulação monitore e avalie sua própria saúde de forma autônoma. Nossa solução integra **telemetria de hardware/simulação**, **análise inteligente de anomalias** e um **painel operacional de controle** para identificar riscos precoces (como exposição à radiação, fadiga cardiovascular e perda óssea) e recomendar contramedidas imediatas.

---

## 🏗️ Arquitetura do Sistema

O projeto segue uma arquitetura distribuída em microsserviços desacoplados, comunicando-se via **REST / JSON**:

```mermaid
graph LR
    subgraph s1["1. Edge & Geração"]
        Hardware["Hardware<br/>(ESP32/Arduino)<br/>ou Simulador"]
    end

    subgraph s3["3. Painel da Missão (mission-control-ui)"]
        Frontend["Dashboard<br/>Astronauta<br/>(HTML +<br/>Tailwind + JS)"]
    end

    subgraph s2["2. Core API & Persistência (astronaut-health-api)"]
        Java["Spring Boot<br/>REST API (Porta<br/>8080)"] <-->|JPA| H2[(Banco H2)]
    end

    subgraph s4["4. Inteligência & Análise (telemetry-analytics)"]
        Python["Motor Python +<br/>Flask<br/>(Análise & Riscos)"]
    end

    %% Conexões de rede REST
    Hardware -->|POST /api/telemetry| Java
    Java -->|GET /api/telemetry/latest| Frontend
    Java -->|POST /api/v1/analyze| Python
    Python -.->|"200 OK (JSON)"| Java
```

## 📦 Repositórios da Organização

O código-fonte está dividido por responsabilidade técnica para permitir desenvolvimento paralelo sem gargalos:

| Repositório | Stack Principal | Responsabilidade |
| :--- | :--- | :--- |
| **[astronaut-health-api](https://github.com/Rapadura-Cosmica/astronaut-health-api)** | Java, Spring Boot, H2 | API REST central que recebe os dados do simulador, faz a persistência do histórico clínico e o despacho de alertas ao painel. |
| **[telemetry-analytics](https://github.com/Rapadura-Cosmica/telemetry-analytics)** | Python, Flask, Pytest | Simulador de telemetria e Motor Clínico responsável por avaliar biometria, classificar nível de risco e gerar recomendações médicas. |
| **[mission-control-ui](https://github.com/Rapadura-Cosmica/mission-control-ui)** | HTML5, Tailwind, JS | Dashboard em tempo real para visualização de sinais vitais, níveis de alerta e protocolos de ação da tripulação. |

## 🔌 Contrato de Integração (JSON)

Para manter todas as frentes sincronizadas, a comunicação segue dois formatos ao longo do fluxo:

### 1. Ingestão de Dados Brutos (Simulador ➔ Core API Java)
Payload enviado pelo Hardware/Simulador com as métricas biométricas coletadas:

```json
{
  "astronautId": "ASTRO-01",
  "timestamp": "2026-11-14T15:30:00Z",
  "heartRate": 118,
  "spo2": 96,
  "radiationDoseMsv": 0.4,
  "sleepHours": 7.2
}
```

### 2. Telemetria Classificada (Análise Python ➔ Java ➔ Dashboard)
Payload enriquecido retornado pelo motor Python após o processamento de risco e geração de recomendações:

```json
{
  "astronautId": "ASTRO-01",
  "timestamp": "2026-11-14T15:30:00Z",
  "heartRate": 118,
  "spo2": 96,
  "radiationDoseMsv": 0.4,
  "sleepHours": 7.2,
  "riskLevel": "WARNING",
  "alertMessage": "Frequência cardíaca elevada detectada. Recomenda-se hidratação e repouso."
}
```

## 👨‍🚀 Equipe Rapadura Côsmica

* **Core API & Persistência (Java / Spring Boot):** [Mateus Queiroz](https://github.com/tue1093) & [Grimaldo Júnior](https://github.com/grimaldojunior0413-cloud)
* **Edge, Hardware & Análise de Anomalias (Python / Flask):** [Murilo Costa](https://github.com/MuriloDev07) & [Gabriel Guedes](https://github.com/usuario-pendente-para-colocar)
* **Interface & Experiência do Astronauta (Front-end):** [Cauã Max](https://github.com/pl4utus)
* **Pesquisa Científica NASA, Documentação & Pitch:** [Davi Duclerc](https://github.com/daviduclerc)

---
Desenvolvido com energia nordestina durante o NASA International Space Apps Challenge 2026.
