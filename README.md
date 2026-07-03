#  Pulse-Ops Dashboard

Language / Idioma:
* [Português (PT-BR)](#-português-pt-br)
* [English (EN-US)](#-english-en-us)

---

## 🇧🇷 Português (PT-BR)

#  Pulse-Ops: Dashboard 

![Go Version](https://img.shields.io/badge/Go-1.22+-00ADD8?style=for-the-badge&logo=go)
![Next.js](https://img.shields.io/badge/Next.js-14+-000000?style=for-the-badge&logo=next.js)
![Angular](https://img.shields.io/badge/Angular-17+-DD0031?style=for-the-badge&logo=angular)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16-4169E1?style=for-the-badge&logo=postgresql)
![Redis](https://img.shields.io/badge/Redis-7+-DC382D?style=for-the-badge&logo=redis)
![WebSocket](https://img.shields.io/badge/WebSocket-Real--time-FF6600?style=for-the-badge&logo=socket.io)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)

O **Pulse-Ops** é uma plataforma de monitoramento e controle de eventos de latência ultrabaixa, projetada para ambientes operacionais críticos. Construído com foco em **sincronização de estado em tempo real** e **integridade operacional**, ele permite que tomadores de decisão visualizem e atuem instantaneamente sobre fluxos de dados massivos.

---

###  O Nome: Por que "Pulse-Ops"?

* **Pulse (Pulso):** Representa o "batimento cardíaco" do sistema. Refere-se à natureza em tempo real da aplicação, monitorando a saúde, o fluxo de dados e os eventos críticos no exato milissegundo em que ocorrem, sem engasgos.
* **Ops (Operações):** Remete à gestão, controle e Engenharia de Confiabilidade (SRE). Não é apenas um painel de visualização passivo, mas uma ferramenta ativa de operação, orquestração e intervenção direta na arquitetura.

###  O Problema (The "Why")

O mercado de engenharia de software atual está saturado de aplicações baseadas em modelos CRUD tradicionais e *polling* (requisições HTTP cíclicas). Em ambientes de alta carga, o *polling* destrói a performance do banco de dados, satura a rede e degrada a experiência do usuário com dados defasados.

O Pulse-Ops foi concebido para resolver desafios reais de **Sistemas Distribuídos**. O objetivo arquitetural é demonstrar o domínio sobre gargalos de rede, concorrência extrema e orquestração de estado complexa, provando que é possível manter consistência de dados, baixa latência e alta disponibilidade simultaneamente.

###  Para que serve?

O Pulse-Ops atua como um **"Command Center"** (Centro de Comando) para operações de alto risco e alto volume. Casos de uso ideais:
* **Fábricas Inteligentes (IoT):** Monitoramento de sensores de maquinário industrial para prevenção de falhas.
* **E-commerce & Fintechs:** Gestão de surtos de transações e pedidos (ex: Black Friday) com detecção de anomalias.
* **Logística:** Controle de frotas e roteamento dinâmico em tempo real.

Ele permite que operadores vejam a realidade do sistema sem atrasos e tomem ações imediatas (como pausar o processamento ou desviar tráfego) com resposta instantânea na interface.

---

###  Diferenciais de Engenharia (Core Features)

- **🔄 Zero-Polling Architecture:** Substituição completa de requisições REST tradicionais por fluxos baseados em eventos via WebSockets, garantindo latência de rede na casa dos milissegundos.
- **✨ Optimistic Updates (A Ilusão da Latência Zero):** Quando o usuário toma uma ação, a UI (via Zustand + TanStack Query) assume o sucesso da operação e atualiza a tela imediatamente. Em caso de falha de rede ou servidor, um `rollback` determinístico restaura o estado anterior de forma invisível.
- **🚨 "Panic Mode" (Circuit Breaker Integrado):** Gatilhos de controle que ativam mecanismos de segurança no back-end. Com um clique, é possível travar o consumo de filas de mensagens do Redis, evitando falhas em cascata no sistema sob stress.
- **🛡️ Idempotência e Confiabilidade:** Tratamento rigoroso no back-end para garantir que comandos reenviados por instabilidade de rede não dupliquem operações críticas.

---

### 🏗️ Arquitetura do Sistema (Detalhada)

O Pulse-Ops segue uma topologia orientada a eventos (Event-Driven Architecture) com camadas bem definidas:

```
┌────────────────────────────────────────────────────────────┐
│                FRONTEND (Next.js / Angular)                │
│  ┌─────────────┐  ┌──────────────┐  ┌──────────────┐      │
│  │  Dashboard  │  │  Gráficos    │  │  Controles   │      │
│  │  em Tempo   │  │  de Alta     │  │  de Operação │      │
│  │  Real       │  │  Performance │  │  (Panic Mode)│      │
│  └──────┬──────┘  └──────┬───────┘  └──────┬───────┘      │
└─────────┼────────────────┼──────────────────┼──────────────┘
          │ WebSocket (wss://)                │ HTTP/2 (fallback)
          │                                   │
┌─────────┼────────────────┼──────────────────┼──────────────┐
│         │  API GATEWAY / WEBSOCKET HUB (Go)                │
│  ┌──────┴─────────────────┴─────────────────┴──────┐       │
│  │  Gorilla WebSocket Server (Goroutines)          │       │
│  │  - Autenticação JWT                             │       │
│  │  - Rate Limiting                                │       │
│  │  - Validação de payload                         │       │
│  └──────┬──────────────────────────────────────────┘       │
└─────────┼──────────────────────────────────────────────────┘
          │ Redis Pub/Sub + Redis Streams
          │
┌─────────┼──────────────────────────────────────────────────┐
│         │        PROCESSING LAYER (Go)                     │
│  ┌──────┴──────────────────────────────────────────┐       │
│  │  Event Processors (Goroutines)                  │       │
│  │  - Consome eventos do Redis                     │       │
│  │  - Persiste no PostgreSQL (transações ACID)     │       │
│  │  - Publica confirmações/atualizações            │       │
│  │  - Garante idempotência (Idempotency Keys)      │       │
│  └─────────────────────────────────────────────────┘       │
└────────────────────────────────────────────────────────────┘
          │
┌─────────┼──────────────────────────────────────────────────┐
│         │        DATA LAYER                                │
│  ┌──────┴──────────────┐  ┌────────────────────────────┐   │
│  │  PostgreSQL 16      │  │  Redis 7+                  │   │
│  │  - Event Store      │  │  - Pub/Sub Channels        │   │
│  │  - State Snapshots  │  │  - Streams (fila durável)  │   │
│  │  - Audit Log        │  │  - Cache de sessão         │   │
│  └─────────────────────┘  └────────────────────────────┘   │
└────────────────────────────────────────────────────────────┘
```

**Fluxo de uma ação crítica:**

1. **Usuário clica** em "Pausar Processamento" no frontend.
2. **Comando via WebSocket** é enviado ao Hub Go, que valida e publica no canal Redis `commands:pause`.
3. **Processador de comandos** (Go) recebe, persiste a intenção no PostgreSQL com status "pending" e emite evento `system:paused` no Redis Pub/Sub.
4. **Hub WebSocket** retransmite para todos os clientes conectados a nova realidade do sistema.
5. **Frontend** aplica Optimistic Update (já mostrava o estado como pausado) e confirma com o evento real, reconciliando se necessário.

---

### 🛠️ Tech Stack Completa

#### Back-end (A "Inteligência" em Go)
| Componente | Tecnologia | Justificativa |
|------------|------------|---------------|
| Linguagem | Go 1.22+ | Concorrência nativa com Goroutines, baixo footprint de memória, performance de rede excepcional |
| WebSocket Server | Gorilla WebSocket | Biblioteca madura, suporte a upgrades e controle de frames |
| Mensageria | Redis Pub/Sub + Redis Streams | Pub/Sub para broadcast em tempo real, Streams para filas persistentes com consumer groups |
| Persistência | PostgreSQL 16 | Transações ACID, JSONB para payloads flexíveis, índices parciais para eventos recentes |
| Autenticação | JWT (golang-jwt) | Tokens leves validados no upgrade do WebSocket, renovação via refresh tokens |
| Observabilidade | Prometheus client + OpenTelemetry | Métricas de Goroutines, latência de Pub/Sub, throughput de eventos |
| Resiliência | Circuit Breaker (gobreaker) | Isola falhas de Redis/PostgreSQL, evita cascata |

#### Front-end Next.js (A "Interação Principal")
| Componente | Tecnologia | Justificativa |
|------------|------------|---------------|
| Framework | Next.js 14 (App Router) | SSR para SEO, RSC para dados iniciais, ótima DX |
| Estado Global | Zustand | Leve, permite atualizações cirúrgicas sem re-renderizações em cascata |
| Mutações Assíncronas | TanStack Query (React Query) | Gerencia cache, optimistic updates, retry e rollback |
| UI/UX | TailwindCSS, Shadcn/UI, Lucide Icons | Estilização rápida, componentes acessíveis, ícones consistentes |
| WebSocket Client | Native WebSocket API | Evita overhead de bibliotecas como Socket.io, controle total sobre frames |

#### Front-end Angular (O "Command Center Enterprise")
| Componente | Tecnologia | Justificativa |
|------------|------------|---------------|
| Framework | Angular 17+ | Estrutura opinativa, ideal para grandes equipes e aplicações de missão crítica |
| Streams | RxJS | Manipulação de milhares de mensagens/seg com operadores como `bufferTime`, `debounce`, `switchMap` |
| Reatividade | Angular Signals | Detecção de mudanças granular sem Zone.js, performance superior em grids e gráficos |
| Visualização | D3.js + Angular Charts | Gráficos customizados de alta performance alimentados diretamente por Signals |

---

### 🅰️ Integração Avançada: Dashboard Enterprise em Angular

Como parte do compromisso com ambientes corporativos de alto desempenho, o Pulse-Ops oferece uma implementação avançada de frontend stateless utilizando **Angular**. Esta é uma alternativa empresarial ao cliente Next.js, otimizada para consumo massivo de dados, manipulação complexa de streams e visualização pesada de métricas.

#### 🏗️ Arquitetura Monorepo

```
pulse-ops/
├── backend-go/             # Inteligência: Go, WebSockets, Redis, Postgres
├── frontend-nextjs/        # UI principal (React 18+, Zustand, TanStack Query)
├── frontend-angular/       # Command Center Enterprise (Angular 17+)
│   ├── src/
│   │   ├── app/
│   │   │   ├── core/       # Serviços globais (WebSocket via RxJS, Telemetria)
│   │   │   │   ├── services/
│   │   │   │   │   ├── websocket.service.ts
│   │   │   │   │   ├── event-bus.service.ts
│   │   │   │   │   └── telemetry.service.ts
│   │   │   │   └── guards/
│   │   │   ├── features/   # Módulos de negócio
│   │   │   │   ├── main-grid/       # Grid de eventos com virtual scroll
│   │   │   │   ├── performance-charts/ # Gráficos de latência/throughput
│   │   │   │   └── panic-controls/  # Controles do Modo Pânico
│   │   │   └── shared/     # Componentes, Diretivas, Pipes reutilizáveis
│   │   │       ├── components/
│   │   │       ├── directives/
│   │   │       └── pipes/
│   │   └── environments/
│   └── angular.json
└── docker-compose.yml
```

#### 🧠 Por que Angular para o Enterprise?

- **RxJS como cidadão de primeira classe:** Orquestra milhares de mensagens WebSocket por segundo com operadores como `bufferTime`, `throttleTime` e `auditTime`, sem bloquear a thread principal.
- **Signals para Reatividade Cirúrgica:** Substitui Zone.js, atualizando apenas os pixels que mudaram em grids com milhares de linhas, eliminando layout thrashing.
- **Injeção de Dependência:** Facilita testes unitários e troca de implementações (ex: mock do WebSocket para desenvolvimento).

#### 📡 Exemplo: Serviço WebSocket com RxJS e Signals

```typescript
// websocket.service.ts
import { Injectable, signal } from '@angular/core';
import { webSocket, WebSocketSubject } from 'rxjs/webSocket';
import { retryWhen, delay, tap } from 'rxjs/operators';

@Injectable({ providedIn: 'root' })
export class WebSocketService {
  private socket$: WebSocketSubject<any>;
  
  // Signal exposto para componentes consumirem sem subscribar manualmente
  public latestEvent = signal<any>(null);

  connect(token: string): void {
    this.socket$ = webSocket({
      url: `wss://api.pulseops.io/ws?token=${token}`,
      openObserver: { next: () => console.log('Canal seguro estabelecido') },
      closeObserver: { next: () => this.reconnect() }
    });

    this.socket$.pipe(
      retryWhen(errors => errors.pipe(delay(1000))),
      tap(event => this.latestEvent.set(event))
    ).subscribe();
  }
  
  sendCommand(command: any) { this.socket$.next(command); }
}
```

#### 🎯 Rollback Determinístico no Angular

Quando um comando de "Panic Mode" é enviado, o frontend Angular imediatamente atualiza a UI para o estado de pausa (Optimistic). Se o servidor retornar erro (ex: `idempotency_key` duplicada), o Signal é revertido:

```typescript
async togglePanicMode() {
  const previousState = this.systemStatus();
  this.systemStatus.set('PAUSED'); // Optimistic
  try {
    await this.api.pauseSystem(idempotencyKey);
  } catch {
    this.systemStatus.set(previousState); // Rollback
  }
}
```

---

### ⚙️ Instalação e Execução Local

#### Pré-requisitos
* Docker & Docker Compose
* Go 1.22+
* Node.js 20+ (para frontends)
* Angular CLI 17+ (para frontend-angular)

#### Passo a Passo

1. **Clone o repositório:**
   ```bash
   git clone https://github.com/kauandias747474-hue/Pulse-Ops.git
   cd Pulse-Ops
   ```

2. **Infraestrutura de apoio:**
   ```bash
   docker-compose up -d postgres redis
   ```

3. **Back-end Go:**
   ```bash
   cd backend-go
   cp .env.example .env   # Configure variáveis
   go mod tidy
   go run cmd/main.go
   ```

4. **Frontend Next.js (Opcional):**
   ```bash
   cd frontend-nextjs
   cp .env.example .env.local
   npm install
   npm run dev
   ```
   Acessível em `http://localhost:3000`

5. **Frontend Angular (Enterprise, Opcional):**
   ```bash
   cd frontend-angular
   npm install
   ng serve
   ```
   Acessível em `http://localhost:4200`

---

### 🧪 Testes

- **Backend Go:**
  ```bash
  cd backend-go
  go test ./... -v -race
  ```
- **Frontend Next.js:**
  ```bash
  cd frontend-nextjs
  npm test -- --watchAll
  ```
- **Frontend Angular:**
  ```bash
  cd frontend-angular
  ng test
  ```

---

### 📊 Observabilidade e Métricas

O Pulse-Ops exporta métricas no formato Prometheus através do endpoint `/metrics` no back-end Go. Um dashboard Grafana de exemplo está disponível em `infra/grafana/dashboards/pulse-ops.json`.

**Métricas-chave expostas:**
- `pulseops_websocket_connections_active` (Gauge)
- `pulseops_events_processed_total` (Counter)
- `pulseops_redis_pubsub_latency_seconds` (Histogram)
- `pulseops_postgres_transaction_duration_seconds` (Summary)

---

### 🗺️ Roadmap

- [x] **Fase 1:** Prova de Conceito (Comunicação Bidirecional + Redis Pub/Sub).
- [ ] **Fase 2:** Implementação completa de Optimistic Updates na Grid principal de dados.
- [ ] **Fase 3:** Autenticação JWT com renovação contínua via WebSocket.
- [ ] **Fase 4:** Dashboard de telemetria com exportação de métricas para Prometheus/Grafana.
- [ ] **Fase 5:** Suporte a clusters de WebSocket com Redis Streams para garantia de entrega.
- [ ] **Fase 6:** Kubernetes Operator para auto-scaling baseado em métricas de eventos.

---

## 🇺🇸 English (EN-US)

#  Pulse-Ops Dashboard

![Go Version](https://img.shields.io/badge/Go-1.22+-00ADD8?style=for-the-badge&logo=go)
![Next.js](https://img.shields.io/badge/Next.js-14+-000000?style=for-the-badge&logo=next.js)
![Angular](https://img.shields.io/badge/Angular-17+-DD0031?style=for-the-badge&logo=angular)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16-4169E1?style=for-the-badge&logo=postgresql)
![Redis](https://img.shields.io/badge/Redis-7+-DC382D?style=for-the-badge&logo=redis)
![WebSocket](https://img.shields.io/badge/WebSocket-Real--time-FF6600?style=for-the-badge&logo=socket.io)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)

**Pulse-Ops** is an ultra-low latency event monitoring and control platform designed for critical operational environments. Built with a focus on **real-time state synchronization** and **operational integrity**, it empowers decision-makers to visualize and instantly act upon massive data streams.

---

###  The Name: Why "Pulse-Ops"?

* **Pulse:** Represents the "heartbeat" of the system. It highlights the real-time nature of the application, monitoring system health, data flow, and critical events in the exact millisecond they occur, without bottlenecks.
* **Ops (Operations):** Stands for management, control, and Site Reliability Engineering (SRE). It implies this is not merely a passive viewing panel, but an active tool for system operation, orchestration, and direct architectural intervention.

###  The Problem (The "Why")

The current software engineering landscape is saturated with standard CRUD applications relying on inefficient `polling` (cyclic HTTP requests). In high-load environments, polling destroys database performance, saturates the network, and degrades user experience with stale data.

Pulse-Ops was conceived to solve real-world **Distributed Systems** challenges. The architectural goal is to demonstrate mastery over network bottlenecks, extreme concurrency, and complex state orchestration, proving it is possible to maintain data consistency, low latency, and high availability simultaneously.

###  What is it for?

Pulse-Ops acts as a **Command Center** for high-stakes, high-volume operations. Ideal use cases:
* **Smart Factories (IoT):** Monitoring industrial machinery sensors for failure prevention.
* **E-commerce & Fintechs:** Managing transaction and order spikes (e.g., Black Friday) with anomaly detection.
* **Logistics:** Real-time fleet control and dynamic routing.

It allows operators to see system reality without delay and take immediate action (such as pausing processing or rerouting traffic) with instantaneous UI feedback.

---

###  Core Engineering Features

- **Zero-Polling Architecture:** Complete replacement of traditional REST requests with event-driven flows via WebSockets, ensuring network latency in the low milliseconds.
- **Optimistic Updates (The Illusion of Zero Latency):** When a user takes action, the UI (via Zustand + TanStack Query) assumes the operation's success and updates the screen immediately. In the event of a network or server failure, a deterministic `rollback` invisibly restores the previous state.
- **"Panic Mode" (Integrated Circuit Breaker):** Control triggers that activate back-end safety mechanisms. With one click, it's possible to halt the consumption of Redis message queues, preventing cascading failures in a stressed system.
- **Idempotency and Reliability:** Strict back-end handling to ensure that commands resent due to network instability do not duplicate critical operations.

---

### 🏗️ System Architecture (Detailed)

Pulse-Ops follows an Event-Driven Architecture with clearly defined layers:

```
┌────────────────────────────────────────────────────────────┐
│                FRONTEND (Next.js / Angular)                │
│  ┌─────────────┐  ┌──────────────┐  ┌──────────────┐      │
│  │  Real-time  │  │  High-Perf   │  │  Operational │      │
│  │  Dashboard  │  │  Charts      │  │  Controls    │      │
│  └──────┬──────┘  └──────┬───────┘  └──────┬───────┘      │
└─────────┼────────────────┼──────────────────┼──────────────┘
          │ WebSocket (wss://)                │ HTTP/2 (fallback)
          │                                   │
┌─────────┼────────────────┼──────────────────┼──────────────┐
│         │  API GATEWAY / WEBSOCKET HUB (Go)                │
│  ┌──────┴─────────────────┴─────────────────┴──────┐       │
│  │  Gorilla WebSocket Server (Goroutines)          │       │
│  │  - JWT Authentication                           │       │
│  │  - Rate Limiting                                │       │
│  │  - Payload Validation                           │       │
│  └──────┬──────────────────────────────────────────┘       │
└─────────┼──────────────────────────────────────────────────┘
          │ Redis Pub/Sub + Redis Streams
          │
┌─────────┼──────────────────────────────────────────────────┐
│         │        PROCESSING LAYER (Go)                     │
│  ┌──────┴──────────────────────────────────────────┐       │
│  │  Event Processors (Goroutines)                  │       │
│  │  - Consume events from Redis                    │       │
│  │  - Persist to PostgreSQL (ACID transactions)    │       │
│  │  - Publish confirmations/updates                │       │
│  │  - Idempotency enforcement (Idempotency Keys)   │       │
│  └─────────────────────────────────────────────────┘       │
└────────────────────────────────────────────────────────────┘
          │
┌─────────┼──────────────────────────────────────────────────┐
│         │        DATA LAYER                                │
│  ┌──────┴──────────────┐  ┌────────────────────────────┐   │
│  │  PostgreSQL 16      │  │  Redis 7+                  │   │
│  │  - Event Store      │  │  - Pub/Sub Channels        │   │
│  │  - State Snapshots  │  │  - Streams (durable queue) │   │
│  │  - Audit Log        │  │  - Session Cache           │   │
│  └─────────────────────┘  └────────────────────────────┘   │
└────────────────────────────────────────────────────────────┘
```

**Critical action flow:**

1. **User clicks** "Pause Processing" in the frontend.
2. **Command via WebSocket** sent to Go Hub, which validates and publishes to Redis channel `commands:pause`.
3. **Command processor** (Go) receives, persists the intent in PostgreSQL with "pending" status, and emits `system:paused` event on Redis Pub/Sub.
4. **WebSocket Hub** broadcasts new system reality to all connected clients.
5. **Frontend** applies Optimistic Update (already displayed as paused) and reconciles with real event if needed.

---

### 🛠️ Full Tech Stack

#### Back-end (The "Intelligence" in Go)
| Component | Technology | Justification |
|-----------|------------|---------------|
| Language | Go 1.22+ | Native concurrency with Goroutines, low memory footprint, exceptional network performance |
| WebSocket Server | Gorilla WebSocket | Mature library, supports upgrades and frame control |
| Messaging | Redis Pub/Sub + Redis Streams | Pub/Sub for real-time broadcast, Streams for persistent queues with consumer groups |
| Persistence | PostgreSQL 16 | ACID transactions, JSONB for flexible payloads, partial indexes for recent events |
| Authentication | JWT (golang-jwt) | Lightweight tokens validated at WebSocket upgrade, renewal via refresh tokens |
| Observability | Prometheus client + OpenTelemetry | Goroutine metrics, Pub/Sub latency, event throughput |
| Resilience | Circuit Breaker (gobreaker) | Isolates Redis/PostgreSQL failures, prevents cascading |

#### Frontend Next.js (The "Primary Interaction")
| Component | Technology | Justification |
|-----------|------------|---------------|
| Framework | Next.js 14 (App Router) | SSR for SEO, RSC for initial data, great DX |
| Global State | Zustand | Lightweight, allows surgical updates without cascading re-renders |
| Async Mutations | TanStack Query (React Query) | Manages cache, optimistic updates, retry and rollback |
| UI/UX | TailwindCSS, Shadcn/UI, Lucide Icons | Rapid styling, accessible components, consistent icons |
| WebSocket Client | Native WebSocket API | Avoids overhead of libraries like Socket.io, full frame control |

#### Frontend Angular (The "Enterprise Command Center")
| Component | Technology | Justification |
|-----------|------------|---------------|
| Framework | Angular 17+ | Opinionated structure, ideal for large teams and mission-critical apps |
| Streams | RxJS | Handles thousands of messages/sec with operators like `bufferTime`, `debounce`, `switchMap` |
| Reactivity | Angular Signals | Granular change detection without Zone.js, superior performance in grids and charts |
| Visualization | D3.js + Angular Charts | Custom high-performance charts fed directly by Signals |

---

### 🅰️ Advanced Integration: Enterprise Angular Dashboard

As part of our commitment to high-performance enterprise environments, Pulse-Ops offers an advanced stateless frontend implementation using **Angular**. This is an enterprise alternative to the Next.js client, optimized for massive data consumption, complex stream manipulation, and heavy metric visualization.

#### 🏗️ Monorepo Architecture

```
pulse-ops/
├── backend-go/             # Intelligence: Go, WebSockets, Redis, Postgres
├── frontend-nextjs/        # Primary UI (React 18+, Zustand, TanStack Query)
├── frontend-angular/       # Enterprise Command Center (Angular 17+)
│   ├── src/
│   │   ├── app/
│   │   │   ├── core/       # Global services (WebSocket via RxJS, Telemetry)
│   │   │   │   ├── services/
│   │   │   │   │   ├── websocket.service.ts
│   │   │   │   │   ├── event-bus.service.ts
│   │   │   │   │   └── telemetry.service.ts
│   │   │   │   └── guards/
│   │   │   ├── features/   # Business modules
│   │   │   │   ├── main-grid/       # Event grid with virtual scroll
│   │   │   │   ├── performance-charts/ # Latency/throughput charts
│   │   │   │   └── panic-controls/  # Panic Mode controls
│   │   │   └── shared/     # Reusable components, directives, pipes
│   │   │       ├── components/
│   │   │       ├── directives/
│   │   │       └── pipes/
│   │   └── environments/
│   └── angular.json
└── docker-compose.yml
```

#### 🧠 Why Angular for the Enterprise?

- **RxJS as a first-class citizen:** Orchestrates thousands of WebSocket messages per second with operators like `bufferTime`, `throttleTime`, and `auditTime`, without blocking the main thread.
- **Signals for Surgical Reactivity:** Replaces Zone.js, updating only the pixels that changed in grids with thousands of rows, eliminating layout thrashing.
- **Dependency Injection:** Facilitates unit testing and swapping implementations (e.g., mock WebSocket for development).

#### 📡 Example: WebSocket Service with RxJS and Signals

```typescript
// websocket.service.ts
import { Injectable, signal } from '@angular/core';
import { webSocket, WebSocketSubject } from 'rxjs/webSocket';
import { retryWhen, delay, tap } from 'rxjs/operators';

@Injectable({ providedIn: 'root' })
export class WebSocketService {
  private socket$: WebSocketSubject<any>;
  
  // Signal exposed for components to consume without manual subscription
  public latestEvent = signal<any>(null);

  connect(token: string): void {
    this.socket$ = webSocket({
      url: `wss://api.pulseops.io/ws?token=${token}`,
      openObserver: { next: () => console.log('Secure channel established') },
      closeObserver: { next: () => this.reconnect() }
    });

    this.socket$.pipe(
      retryWhen(errors => errors.pipe(delay(1000))),
      tap(event => this.latestEvent.set(event))
    ).subscribe();
  }
  
  sendCommand(command: any) { this.socket$.next(command); }
}
```

#### 🎯 Deterministic Rollback in Angular

When a "Panic Mode" command is sent, the Angular frontend immediately updates the UI to the paused state (Optimistic). If the server returns an error (e.g., duplicate `idempotency_key`), the Signal is rolled back:

```typescript
async togglePanicMode() {
  const previousState = this.systemStatus();
  this.systemStatus.set('PAUSED'); // Optimistic
  try {
    await this.api.pauseSystem(idempotencyKey);
  } catch {
    this.systemStatus.set(previousState); // Rollback
  }
}
```

---

### ⚙️ Local Setup & Installation

#### Prerequisites
* Docker & Docker Compose
* Go 1.22+
* Node.js 20+ (for frontends)
* Angular CLI 17+ (for frontend-angular)

#### Step-by-Step

1. **Clone the repository:**
   ```bash
   git clone https://github.com/kauandias747474-hue/Pulse-Ops.git
   cd Pulse-Ops
   ```

2. **Supporting infrastructure:**
   ```bash
   docker-compose up -d postgres redis
   ```

3. **Go Backend:**
   ```bash
   cd backend-go
   cp .env.example .env   # Configure variables
   go mod tidy
   go run cmd/main.go
   ```

4. **Next.js Frontend (Optional):**
   ```bash
   cd frontend-nextjs
   cp .env.example .env.local
   npm install
   npm run dev
   ```
   Available at `http://localhost:3000`

5. **Angular Frontend (Enterprise, Optional):**
   ```bash
   cd frontend-angular
   npm install
   ng serve
   ```
   Available at `http://localhost:4200`

---

### 🧪 Testing

- **Backend Go:**
  ```bash
  cd backend-go
  go test ./... -v -race
  ```
- **Frontend Next.js:**
  ```bash
  cd frontend-nextjs
  npm test -- --watchAll
  ```
- **Frontend Angular:**
  ```bash
  cd frontend-angular
  ng test
  ```

---

### 📊 Observability and Metrics

Pulse-Ops exports Prometheus metrics via the `/metrics` endpoint in the Go backend. A sample Grafana dashboard is available at `infra/grafana/dashboards/pulse-ops.json`.

**Key metrics exposed:**
- `pulseops_websocket_connections_active` (Gauge)
- `pulseops_events_processed_total` (Counter)
- `pulseops_redis_pubsub_latency_seconds` (Histogram)
- `pulseops_postgres_transaction_duration_seconds` (Summary)

---

### 🗺️ Roadmap

- [x] **Phase 1:** Proof of Concept (Bidirectional Communication + Redis Pub/Sub).
- [ ] **Phase 2:** Full implementation of Optimistic Updates on the main data grid.
- [ ] **Phase 3:** JWT Authentication with continuous renewal via WebSocket.
- [ ] **Phase 4:** Telemetry dashboard with metrics export to Prometheus/Grafana.
- [ ] **Phase 5:** WebSocket cluster support with Redis Streams for guaranteed delivery.
- [ ] **Phase 6:** Kubernetes Operator for auto-scaling based on event metrics.

---

## 🤝 Contributing / Contribuição
Pull Requests are welcome. For major changes, please open an issue first to discuss what you would like to change. / Pull Requests são bem-vindos. Para mudanças maiores, abra uma issue primeiro para discussão.

## 📄 License / Licença
Distributed under the MIT License. See `LICENSE` for more information. / Distribuído sob a licença MIT. Veja `LICENSE` para mais informações.
```
