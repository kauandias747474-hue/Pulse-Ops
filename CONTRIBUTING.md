# 🤝 Contribuindo com o Pulse-Ops / Contributing to Pulse-Ops / Contribuyendo con Pulse-Ops

Idiomas / Languages / Idiomas:
* [Português (PT-BR)](#-português-pt-br)
* [English (EN-US)](#-english-en-us)
* [Español (ES)](#-español-es)

---

## 🇧🇷 Português (PT-BR)

Obrigado pelo interesse em contribuir com o **Pulse-Ops**! Este documento descreve as diretrizes para colaborar com o projeto, seja reportando problemas, sugerindo melhorias ou enviando código.

### 📜 Código de Conduta

Este projeto adere ao [Contributor Covenant](https://www.contributor-covenant.org/version/2/1/code_of_conduct/). Ao participar, você concorda em seguir seus termos. Denúncias de comportamento inadequado podem ser enviadas para [inserir email de contato].

---

### 🧭 Como posso contribuir?

#### 🐞 Reportando Bugs

1. Verifique se o bug já não foi reportado pesquisando nas [issues](https://github.com/kauandias747474-hue/Pulse-Ops/issues).
2. Abra uma nova issue usando o template "Bug Report", incluindo:
   - Descrição clara do problema.
   - Passos para reproduzir.
   - Comportamento esperado vs. atual.
   - Logs, screenshots e detalhes do ambiente (Go, Node, navegador, etc.).

#### 💡 Sugerindo Melhorias

- Use o template "Feature Request" para propor novas funcionalidades ou mudanças.
- Descreva o problema que sua ideia resolve e como ela se alinha com a arquitetura de tempo real do Pulse-Ops.
- Se possível, esboce uma solução técnica (ex.: fluxo de WebSocket, modificação no Redis Streams).

#### 🛠️ Contribuindo com Código

Quer implementar algo? Siga os passos abaixo para configurar o ambiente e enviar um Pull Request.

---

### ⚙️ Configuração do Ambiente de Desenvolvimento

#### Pré-requisitos

- Go 1.22+
- Node.js 20+ (para frontends)
- Angular CLI 17+ (para o frontend Angular)
- Docker & Docker Compose
- Git

#### Instalação local

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
   cp .env.example .env
   go mod tidy
   go run cmd/main.go
   ```

4. **Frontend Next.js (opcional):**
   ```bash
   cd frontend-nextjs
   cp .env.example .env.local
   npm install
   npm run dev
   ```

5. **Frontend Angular (opcional):**
   ```bash
   cd frontend-angular
   npm install
   ng serve
   ```

---

### 🌳 Fluxo de Trabalho

1. **Crie um branch** a partir de `main` (ou `develop`, se existir):
   ```bash
   git checkout -b feat/minha-funcionalidade
   # ou fix/descricao-do-bug, docs/atualizacao-readme, etc.
   ```

2. **Siga o estilo de código:**
   - **Go:** Formate com `gofmt` e utilize `golangci-lint` para análise estática.
   - **Next.js/React:** Utilize ESLint + Prettier configurados no projeto.
   - **Angular:** Siga as regras do Angular ESLint e use `ng lint`.

3. **Commits semânticos:**
   Utilize [Conventional Commits](https://www.conventionalcommits.org/pt-br/):
   - `feat:` nova funcionalidade
   - `fix:` correção de bug
   - `docs:` documentação
   - `refactor:` refatoração
   - `test:` adição/modificação de testes
   - `chore:` tarefas de build, dependências, etc.

4. **Escreva testes** para a sua contribuição. Veja a seção de testes abaixo.

5. **Atualize a documentação** se necessário (comentários, README, AsyncAPI, ADRs).

6. **Abra um Pull Request** para `main`. No PR, explique o que foi feito, motive as decisões e referencie a issue relacionada.

---

### ✅ Testes

Garanta que todos os testes passem antes de enviar seu PR:

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

Idealmente, cubra novas funcionalidades com testes unitários e/ou de integração. Para mudanças no WebSocket ou Redis, considere testes de contrato ou simulação de eventos.

---

### 📄 Documentação

- Comentários de código são bem-vindos, especialmente em Go para funções exportadas.
- Para mudanças arquiteturais, considere adicionar um ADR em `/docs/adr`.
- Caso modifique a API WebSocket, atualize a especificação AsyncAPI (se disponível).

---

### 💬 Comunicação

- Dúvidas? Use as [discussions](https://github.com/kauandias747474-hue/Pulse-Ops/discussions) do GitHub.
- Issues são para bugs e features; discussões abertas são para brainstorm e suporte.

Agradecemos sua colaboração! 🚀

---

## 🇺🇸 English (EN-US)

Thank you for your interest in contributing to **Pulse-Ops**! This document outlines the guidelines for collaborating on the project, whether by reporting issues, suggesting improvements, or submitting code.

### 📜 Code of Conduct

This project adheres to the [Contributor Covenant](https://www.contributor-covenant.org/version/2/1/code_of_conduct/). By participating, you agree to follow its terms. Reports of unacceptable behavior can be sent to [insert contact email].

---

### 🧭 How Can I Contribute?

#### 🐞 Reporting Bugs

1. Check if the bug has already been reported by searching the [issues](https://github.com/kauandias747474-hue/Pulse-Ops/issues).
2. Open a new issue using the "Bug Report" template, including:
   - Clear description of the problem.
   - Steps to reproduce.
   - Expected vs. actual behavior.
   - Logs, screenshots, and environment details (Go, Node, browser, etc.).

#### 💡 Suggesting Enhancements

- Use the "Feature Request" template to propose new features or changes.
- Describe the problem your idea solves and how it aligns with Pulse-Ops' real‑time architecture.
- If possible, sketch a technical solution (e.g., WebSocket flow, Redis Streams modifications).

#### 🛠️ Contributing Code

Ready to implement something? Follow the steps below to set up your environment and submit a Pull Request.

---

### ⚙️ Development Environment Setup

#### Prerequisites

- Go 1.22+
- Node.js 20+ (for frontends)
- Angular CLI 17+ (for Angular frontend)
- Docker & Docker Compose
- Git

#### Local installation

1. **Clone the repository:**
   ```bash
   git clone https://github.com/kauandias747474-hue/Pulse-Ops.git
   cd Pulse-Ops
   ```

2. **Supporting infrastructure:**
   ```bash
   docker-compose up -d postgres redis
   ```

3. **Go backend:**
   ```bash
   cd backend-go
   cp .env.example .env
   go mod tidy
   go run cmd/main.go
   ```

4. **Next.js frontend (optional):**
   ```bash
   cd frontend-nextjs
   cp .env.example .env.local
   npm install
   npm run dev
   ```

5. **Angular frontend (optional):**
   ```bash
   cd frontend-angular
   npm install
   ng serve
   ```

---

### 🌳 Workflow

1. **Create a branch** from `main` (or `develop`, if it exists):
   ```bash
   git checkout -b feat/my-feature
   # or fix/bug-description, docs/readme-update, etc.
   ```

2. **Follow the code style:**
   - **Go:** Format with `gofmt` and use `golangci-lint` for static analysis.
   - **Next.js/React:** Use the project's ESLint + Prettier configuration.
   - **Angular:** Follow Angular ESLint rules and run `ng lint`.

3. **Semantic commits:**
   Use [Conventional Commits](https://www.conventionalcommits.org/):
   - `feat:` new feature
   - `fix:` bug fix
   - `docs:` documentation
   - `refactor:` code refactoring
   - `test:` adding/updating tests
   - `chore:` build tasks, dependencies, etc.

4. **Write tests** for your contribution. See the testing section below.

5. **Update documentation** if necessary (comments, README, AsyncAPI, ADRs).

6. **Open a Pull Request** against `main`. In the PR, explain what was done, motivate your decisions, and reference the related issue.

---

### ✅ Testing

Ensure all tests pass before submitting your PR:

- **Go backend:**
  ```bash
  cd backend-go
  go test ./... -v -race
  ```

- **Next.js frontend:**
  ```bash
  cd frontend-nextjs
  npm test -- --watchAll
  ```

- **Angular frontend:**
  ```bash
  cd frontend-angular
  ng test
  ```

Ideally, cover new features with unit and/or integration tests. For changes involving WebSocket or Redis, consider contract tests or event simulation.

---

### 📄 Documentation

- Code comments are welcome, especially for exported functions in Go.
- For architectural changes, consider adding an ADR in `/docs/adr`.
- If you modify the WebSocket API, update the AsyncAPI specification (if available).

---

### 💬 Communication

- Questions? Use GitHub [discussions](https://github.com/kauandias747474-hue/Pulse-Ops/discussions).
- Issues are for bugs and features; open discussions are for brainstorming and support.

We appreciate your collaboration! 🚀

---

## 🇪🇸 Español (ES)

¡Gracias por tu interés en contribuir con **Pulse-Ops**! Este documento describe las pautas para colaborar en el proyecto, ya sea reportando problemas, sugiriendo mejoras o enviando código.

### 📜 Código de Conducta

Este proyecto se adhiere al [Contributor Covenant](https://www.contributor-covenant.org/version/2/1/code_of_conduct/). Al participar, aceptas seguir sus términos. Los informes de comportamiento inaceptable pueden enviarse a [insertar correo de contacto].

---

### 🧭 ¿Cómo puedo contribuir?

#### 🐞 Reportando Errores

1. Verifica si el error ya fue reportado buscando en las [issues](https://github.com/kauandias747474-hue/Pulse-Ops/issues).
2. Abre una nueva issue usando la plantilla "Bug Report", incluyendo:
   - Descripción clara del problema.
   - Pasos para reproducirlo.
   - Comportamiento esperado vs. real.
   - Logs, capturas de pantalla y detalles del entorno (Go, Node, navegador, etc.).

#### 💡 Sugiriendo Mejoras

- Usa la plantilla "Feature Request" para proponer nuevas funcionalidades o cambios.
- Describe el problema que tu idea resuelve y cómo se alinea con la arquitectura en tiempo real de Pulse-Ops.
- Si es posible, esboza una solución técnica (ej.: flujo de WebSocket, modificaciones en Redis Streams).

#### 🛠️ Contribuyendo con Código

¿Quieres implementar algo? Sigue los pasos a continuación para configurar tu entorno y enviar un Pull Request.

---

### ⚙️ Configuración del Entorno de Desarrollo

#### Requisitos previos

- Go 1.22+
- Node.js 20+ (para los frontends)
- Angular CLI 17+ (para el frontend Angular)
- Docker & Docker Compose
- Git

#### Instalación local

1. **Clona el repositorio:**
   ```bash
   git clone https://github.com/kauandias747474-hue/Pulse-Ops.git
   cd Pulse-Ops
   ```

2. **Infraestructura de apoyo:**
   ```bash
   docker-compose up -d postgres redis
   ```

3. **Backend Go:**
   ```bash
   cd backend-go
   cp .env.example .env
   go mod tidy
   go run cmd/main.go
   ```

4. **Frontend Next.js (opcional):**
   ```bash
   cd frontend-nextjs
   cp .env.example .env.local
   npm install
   npm run dev
   ```

5. **Frontend Angular (opcional):**
   ```bash
   cd frontend-angular
   npm install
   ng serve
   ```

---

### 🌳 Flujo de Trabajo

1. **Crea una rama** desde `main` (o `develop`, si existe):
   ```bash
   git checkout -b feat/mi-funcionalidad
   # o fix/descripcion-del-bug, docs/actualizacion-readme, etc.
   ```

2. **Sigue el estilo de código:**
   - **Go:** Formatea con `gofmt` y usa `golangci-lint` para análisis estático.
   - **Next.js/React:** Utiliza la configuración de ESLint + Prettier del proyecto.
   - **Angular:** Sigue las reglas de Angular ESLint y ejecuta `ng lint`.

3. **Commits semánticos:**
   Usa [Conventional Commits](https://www.conventionalcommits.org/es/):
   - `feat:` nueva funcionalidad
   - `fix:` corrección de error
   - `docs:` documentación
   - `refactor:` refactorización
   - `test:` adición/modificación de pruebas
   - `chore:` tareas de build, dependencias, etc.

4. **Escribe pruebas** para tu contribución. Consulta la sección de pruebas más abajo.

5. **Actualiza la documentación** si es necesario (comentarios, README, AsyncAPI, ADRs).

6. **Abre un Pull Request** hacia `main`. En el PR, explica qué se hizo, motiva las decisiones y referencia la issue relacionada.

---

### ✅ Pruebas

Asegúrate de que todas las pruebas pasen antes de enviar tu PR:

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

Idealmente, cubre las nuevas funcionalidades con pruebas unitarias y/o de integración. Para cambios en WebSocket o Redis, considera pruebas de contrato o simulación de eventos.

---

### 📄 Documentación

- Los comentarios en el código son bienvenidos, especialmente en Go para funciones exportadas.
- Para cambios arquitectónicos, considera agregar un ADR en `/docs/adr`.
- Si modificas la API WebSocket, actualiza la especificación AsyncAPI (si está disponible).

---

### 💬 Comunicación

- ¿Preguntas? Usa las [discussions](https://github.com/kauandias747474-hue/Pulse-Ops/discussions) de GitHub.
- Las issues son para errores y funcionalidades; las discusiones abiertas son para lluvia de ideas y soporte.

¡Agradecemos tu colaboración! 🚀
```
