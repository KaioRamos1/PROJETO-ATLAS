# Projeto ATLAS — Arquitetura de Rede e Segurança

> Arquitetura de referência para redes corporativas com foco em segmentação, visibilidade e resposta a incidentes.

## 🎯 Objetivos
- Minimizar superfície de ataque e isolar domínios de confiança.
- Padronizar controles (perímetro, interno e identidade).
- Centralizar **logs** em **SIEM/SOC** e orquestrar resposta via **Cloud Services Incident Response**.
- Habilitar trilhas de auditoria e governança de acesso (SSO/MFA).

---

## 🗺️ Arquitetura (Visão Geral)

> O diagrama abaixo é renderizado automaticamente pelo GitHub via **Mermaid**.  
> Linhas **contínuas** = tráfego/fluxo; **tracejadas** = **logs/telemetria**; **pontos** vermelhos = ameaças.

```mermaid
flowchart TB
    %% --- NODES ---
    subgraph InternetZone[Internet / Ameaças Externas]
      INET(Internet):::infra
      SQLI[[SQL Injection]]:::threat
      PORTSCAN[[Port Scan]]:::threat
    end

    DMZ(DMZ<br/>(Filtered)):::infra
    FW(Firewall Perímetro<br/>(Inspected)):::control
    NET(Rede Interna):::infra

    WIFI(WiFi / Guest<br/>VLAN 20 · Isolated):::infra
    DC(Data Center):::infra
    BKP(Backup Storage):::cloud
    DB(Banco de Dados):::infra

    USERS(Usuários):::users
    WS(Estados de Trabalho):::users
    MOB(Dispositivos Móveis):::users

    IAM(IAM Platform<br/>SSO/MFA):::control
    SIEM(SIEM / SOC):::control
    IR(Cloud Services<br/>Incident Response):::cloud

    %% --- FLOWS (DATA/TRÁFEGO) ---
    INET --> DMZ --> FW --> NET
    NET --> USERS
    USERS --> WS
    USERS --> MOB
    NET --> WIFI
    NET --> DC --> DB
    DC --> BKP

    %% --- Ameaças direcionadas ---
    SQLI -.-> DB
    PORTSCAN -.-> IR

    %% --- Integrações de Segurança (LOGS) ---
    FW -. Logs .-> SIEM
    NET -. Logs .-> SIEM
    DC -. Logs .-> SIEM
    DB -. Logs .-> SIEM
    WS -. Logs .-> SIEM
    MOB -. Logs .-> SIEM
    IAM -. Logs .-> SIEM
    SIEM -->|Alerts| IR
    NET -->|Encrypted*| IAM
    USERS -->|Auth| IAM

    %% --- STYLES ---
    classDef threat fill:#ff5b5b,stroke:#b30000,color:#fff,font-weight:bold;
    classDef control fill:#6aa5ff,stroke:#2b6bff,color:#fff;
    classDef infra fill:#3ddc97,stroke:#2ca26f,color:#fff;
    classDef users fill:#d66bff,stroke:#a445d6,color:#fff;
    classDef cloud fill:#5fd1ff,stroke:#2699c7,color:#003b4f;
