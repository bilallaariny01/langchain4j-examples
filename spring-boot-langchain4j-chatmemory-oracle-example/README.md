# Spring Boot LangChain4j Oracle Chat Memory (Auto-Configuration)

Spring Boot example that auto-configures a LangChain4j assistant with Oracle-backed chat memory persistence.

This app demonstrates:

- `@AiService`-driven assistant creation (no manual `AiServices.builder(...)`)
- Ollama chat model auto-configuration
- `MessageWindowChatMemory` auto-configuration
- Oracle `ChatMemoryStore` auto-configuration and persistence

## Requirements

- Java 21+
- Maven 3.9+
- Oracle Database access (JDBC URL, username, password)
- Ollama server (local or remote)
- Local sibling module `../langchain4j-oracle` available and installed

## Installation

Install the local Oracle integration module used by this example:

```bash
cd ../langchain4j-oracle
mvn -DskipTests install
cd ../spring-boot-langchain4j-chatmemory-oracle-example
```

Build this app:

```bash
mvn -DskipTests compile
```

## Quick Start

Set environment variables:

```bash
export OLLAMA_BASE_URL=http://localhost:11434
export OLLAMA_MODEL_NAME=qwen3:8b

export ORACLE_JDBC_URL='jdbc:oracle:thin:@YOUR_DB_ALIAS?TNS_ADMIN=/absolute/path/to/Wallet_MemoryStore'
export ORACLE_JDBC_USER='YOUR_DB_USER'
export ORACLE_JDBC_PASSWORD='YOUR_DB_PASSWORD'

# Optional
export ORACLE_CHAT_MEMORY_TABLE=chat_memory_test
```

Run:

```bash
mvn spring-boot:run
```

CLI usage:
- Type your message and press Enter
- Type `exit` or `quit` to stop

## Auto-Configured Components

### Assistant

The `@AiService` interface is auto-implemented and injected:
- `src/main/java/dev/langchain4j/Assistant.java`

### Chat Model

Ollama chat model is configured from:
- `langchain4j.ollama.chat-model.*`

### Chat Memory

Message window memory is configured from:
- `langchain4j.chat-memory.type`
- `langchain4j.chat-memory.max-messages`

### Oracle Chat Memory Store

Oracle-backed persistence is configured from:
- `langchain4j.community.oracle.chat-memory.*`

## Configuration Properties

The app configuration lives in `src/main/resources/application.yml`.

### Spring DataSource

| Property | Default in this app | Description |
| --- | --- | --- |
| `spring.datasource.url` | `${ORACLE_JDBC_URL:${url}}` | Oracle JDBC URL. |
| `spring.datasource.username` | `${ORACLE_JDBC_USER:${user}}` | Oracle DB username. |
| `spring.datasource.password` | `${ORACLE_JDBC_PASSWORD:${password}}` | Oracle DB password. |
| `spring.datasource.driver-class-name` | `oracle.jdbc.OracleDriver` | Oracle JDBC driver class. |

Legacy fallbacks supported by this app:
- `url` (fallback for `ORACLE_JDBC_URL`)
- `user` (fallback for `ORACLE_JDBC_USER`)
- `password` (fallback for `ORACLE_JDBC_PASSWORD`)

### Ollama Model

Prefix: `langchain4j.ollama.chat-model`

| Property | Default in this app | Description |
| --- | --- | --- |
| `base-url` | `${OLLAMA_BASE_URL:http://localhost:11434}` | Ollama endpoint. |
| `model-name` | `${OLLAMA_MODEL_NAME:qwen3:8b}` | Model used for chat completion. |

### Chat Memory Window

Prefix: `langchain4j.chat-memory`

| Property | Default in this app | Description |
| --- | --- | --- |
| `type` | `message-window` | Chat memory strategy used by the assistant. |
| `max-messages` | `20` | Max messages kept in window before eviction. |

### Oracle Chat Memory Store

Prefix: `langchain4j.community.oracle.chat-memory`

| Property | Default in this app | Description |
| --- | --- | --- |
| `enabled` | `true` | Enables Oracle chat memory auto-configuration. |
| `table-name` | `${ORACLE_CHAT_MEMORY_TABLE:chat_memory_test}` | Oracle table used for persisted memory entries. |
| `ttl` | implementation default | Optional time-to-live for memory entries (if set in your starter/library version). |

Duration examples for `ttl`:
- ISO-8601: `PT1H`, `PT24H`
- Simple suffixes: `30s`, `5m`, `1h`, `7d`

## Full Example `application.yml`

```yaml
spring:
  datasource:
    url: ${ORACLE_JDBC_URL:${url}}
    username: ${ORACLE_JDBC_USER:${user}}
    password: ${ORACLE_JDBC_PASSWORD:${password}}
    driver-class-name: oracle.jdbc.OracleDriver

langchain4j:
  ollama:
    chat-model:
      base-url: ${OLLAMA_BASE_URL:http://localhost:11434}
      model-name: ${OLLAMA_MODEL_NAME:qwen3:8b}
  chat-memory:
    type: message-window
    max-messages: 20
  community:
    oracle:
      chat-memory:
        enabled: true
        table-name: ${ORACLE_CHAT_MEMORY_TABLE:chat_memory_test}
        # Optional in versions that support it:
        # ttl: 7d
```

## Disabling Oracle Chat Memory

Disable Oracle chat memory store auto-configuration:

```yaml
langchain4j:
  community:
    oracle:
      chat-memory:
        enabled: false
```

## Project Layout

```text
src/main/java/dev/langchain4j/
  Assistant.java
  ChatMemoryOracleApplication.java
  Demotools.java

src/main/resources/
  application.yml
```
