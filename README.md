# Route-53-Failover-Routing-AWS-


## 📌 Visão geral

Este projeto demonstra a implementação de uma arquitetura de **alta disponibilidade (High Availability)** utilizando o **Amazon Route 53 com Failover Routing Policy**, integrado com **Amazon EC2, Health Checks e Amazon SNS**.

O objetivo é garantir que uma aplicação web continue disponível mesmo em caso de falha de uma instância principal.

---

## 🧠 Conceitos aplicados

Durante este laboratório foram aplicados os seguintes conceitos:

* DNS com **Route 53**
* Políticas de roteamento **Failover (Primary/Secondary)**
* Monitoramento com **Health Checks**
* Integração com **CloudWatch Alarm**
* Notificações via **Amazon SNS**
* Alta disponibilidade em múltiplas Availability Zones (AZs)

---

## 🏗️ Arquitetura da solução

A arquitetura é composta por:

* **CafeInstance1 (Primária)** → Instância principal em uma AZ
* **CafeInstance2 (Secundária)** → Instância de backup em outra AZ
* **Route 53** → Gerencia o tráfego DNS
* **Health Check** → Monitora a instância primária
* **SNS Topic** → Envia notificações quando falhas são detectadas

Fluxo:

1. Usuário acessa o domínio via Route 53
2. O tráfego é direcionado para a instância primária
3. O Health Check monitora continuamente o endpoint `/cafe`
4. Em caso de falha:

   * Health Check muda para **Unhealthy**
   * Route 53 ativa o failover automaticamente
   * Tráfego é redirecionado para a instância secundária
   * SNS envia notificação por e-mail

---

## ⚙️ Pré-requisitos do ambiente

* 2 instâncias EC2 com Apache + aplicação LAMP já configurada
* Instâncias em diferentes Availability Zones
* Acesso ao AWS Route 53
* Permissões para criar:

  * Health Checks
  * Hosted Zones
  * SNS Topics

---

## 🧪 Implementação passo a passo

---

### 1. Validação das instâncias EC2

Verificações realizadas:

* CafeInstance1 (Primária) em execução
* CafeInstance2 (Secundária) em execução
* Aplicação acessível via IP público:

  * `http://<IP>/cafe/`

---

### 2. Criação do Health Check

No Amazon Route 53:

* **Name:** Primary-Website-Health
* **Type:** Endpoint
* **Protocol:** HTTP
* **IP address:** IP da CafeInstance1
* **Path:** `/cafe`
* **Request interval:** 10 seconds (Fast)
* **Failure threshold:** 2

📌 Resultado: monitoramento ativo da instância primária.

---

### 3. Configuração de alerta SNS

* Criação de um **SNS Topic**:

  * Name: `Primary-Website-Health`
* Adição de e-mail como endpoint
* Confirmação da assinatura via e-mail

📌 Resultado: notificações automáticas em caso de falha.

---

### 4. Configuração do Route 53 (Failover Routing)

Na Hosted Zone do domínio:

#### 🔵 Registro Primário (Primary)

* Name: `www`
* Type: A
* Value: IP da CafeInstance1
* Routing policy: Failover
* Failover type: Primary
* Health Check: habilitado (Primary-Website-Health)
* TTL: 15 segundos

---

#### 🟡 Registro Secundário (Secondary)

* Name: `www`
* Type: A
* Value: IP da CafeInstance2
* Routing policy: Failover
* Failover type: Secondary
* Health Check: NÃO configurado
* TTL: 15 segundos

---

## 🧪 Teste de funcionamento

### Teste inicial (normal)

* Acesso ao domínio:

```
http://www.<dominio>.vocareum.training/cafe/
```

* Resposta da CafeInstance1 (primária)

---

### Simulação de falha

* Instância CafeInstance1 foi parada manualmente
* Health Check detectou falha após ~1–2 minutos
* Status alterado para **Unhealthy**

---

### Resultado do Failover

* Route 53 automaticamente redirecionou o tráfego para:

  * CafeInstance2 (secundária)
* Aplicação permaneceu acessível sem interrupção

---

## 📊 Resultados obtidos

✔ Alta disponibilidade implementada com sucesso
✔ Failover automático funcionando corretamente
✔ Monitoramento contínuo via Health Check
✔ Notificações configuradas via SNS
✔ Redundância entre Availability Zones

---

## 🚀 Conclusão

Este laboratório demonstra uma arquitetura fundamental para sistemas em produção na AWS, garantindo:

* Resiliência contra falhas de instâncias
* Continuidade de serviço
* Automação de failover sem intervenção manual

O uso combinado de Route 53, Health Checks e SNS permite construir soluções altamente confiáveis e escaláveis.

---

## 📌 Tecnologias utilizadas

* Amazon EC2
* Amazon Route 53
* Amazon CloudWatch
* Amazon SNS
* Linux + Apache (LAMP Stack)
