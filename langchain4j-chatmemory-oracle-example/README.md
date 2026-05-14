# LangChain4j Oracle ChatMemory Sample

This project demonstrates:

- `MessageWindowChatMemory` with Oracle-backed persistence (`OracleMemoryStore` from `langchain4j-oracle`)
- Tool-enabled assistant (`Demotools`)
- Custom metadata message (`CustomMessage`) added to memory

Main sample entrypoint:

- `src/main/sample/java/dev/langchain4j/Main.java`

## What You Need

1. Java 21
2. Maven 3.9+
3. Oracle DB access (connection URL, user, password)
4. Ollama running locally or remotely with model `qwen3:8b`
5. Sibling project available at `../langchain4j-oracle` (already in your workspace)

## Required Environment Variables

Set these before running:

```bash
export OLLAMA_BASE_URL=http://localhost:11434
export url='jdbc:oracle:thin:@YOUR_DB_ALIAS?TNS_ADMIN=/absolute/path/to/Wallet_MemoryStore'
export user='YOUR_DB_USER'
export password='YOUR_DB_PASSWORD'
```

Notes:

- Variable names must be exactly: `OLLAMA_BASE_URL`, `url`, `user`, `password`
- `OracleWalletDataSourceFactory` reads these exact names

## Build Oracle Integration Module (one-time or after changes)

From this project root:

```bash
cd ../langchain4j-oracle
mvn -DskipTests install
cd ../langchain4j-Chatmemory
```

This installs `dev.langchain4j:langchain4j-oracle:1.12.0-SNAPSHOT` into your local Maven repository.

## Build This Project

```bash
mvn -DskipTests compile
```

## Run The Sample

### IntelliJ (recommended)

1. Open `src/main/sample/java/dev/langchain4j/Main.java`
2. Run `Main.main()`
3. Chat in terminal, type `exit` or `quit` to stop


