## Speedtest Logger: Ferramenta de Registro de Resultados de Speedtest em JSON

Este script Bash automatiza a execução do Speedtest e registra os resultados em formato JSON em um único arquivo, facilitando o acompanhamento do desempenho da sua conexão ao longo do tempo.

### Recursos

*   Executa o Speedtest com as opções `-f json` (formato JSON) e `-p no` (sem progresso).
*   Armazena os resultados em um arquivo JSON chamado `speedtest_results.json`.
*   Adiciona automaticamente colchetes `[]` para criar um array JSON válido.
*   Formata os resultados para garantir a consistência do JSON.
*   Possui controle de exceção para lidar com o caso do arquivo de log não existir.

### Pré-requisitos

*   Bash (interpretador de comandos padrão em muitos sistemas Linux e macOS)
*   Speedtest CLI (`speedtest`)
*   jq (processador de JSON)

### Instalação

1.  **Speedtest CLI:**
    ```bash
    sudo apt install speedtest
    ```

2.  **jq:**

    Para instalar o jq, você pode utilizar o gerenciador de pacotes do seu sistema operacional. No caso do Debian/Ubuntu:

    ```bash
    sudo apt-get install jq
    ```

    Para outros sistemas operacionais, consulte a documentação oficial do jq:

    *   **Documentação do jq:** [https://stedolan.github.io/jq/](https://stedolan.github.io/jq/)

### Uso

**Opção 1: Script `speedtest_log.sh`**

1.  Salve o código abaixo como `speedtest_log.sh`:

```bash
#!/bin/bash

LOG_FILE="speedtest_results.json"

if [ ! -f "$LOG_FILE" ]; then
    echo "[" > "$LOG_FILE"
fi

result=$(speedtest -f json -p no)
formatted_result=$(echo "$result" | jq '. | .')

echo "$formatted_result," >> "$LOG_FILE"
```

2.  Dê permissão de execução ao script:

```bash
chmod +x speedtest_log.sh
```

3.  Execute o script:

```bash
./speedtest_log.sh
```

**Opção 2: Alias (apenas Bash)**

1.  Adicione o seguinte alias ao seu arquivo de configuração do shell (`.bashrc` ou `.zshrc`):

```bash
alias speedtest_log='if [ ! -f speedtest_results.json ]; then echo "[" > speedtest_results.json; fi; speedtest -f json -p no | jq ". | ." >> speedtest_results.json'
```

2.  Execute o comando:

```bash
speedtest_log
```

### Exemplo de Saída (speedtest_results.json)

```json
[
  {"type":"result", "timestamp":"2024-07-23T20:03:08Z", ...},
  {"type":"result", "timestamp":"2024-07-23T20:15:32Z", ...},
  {"type":"result", "timestamp":"2024-07-23T20:28:17Z", ...}
]
```

### Agendamento Automático (Opcional)

Você pode usar o `cron` para agendar a execução do script em intervalos regulares. Por exemplo, para executar o script a cada hora:

```bash
crontab -e
```

Adicione a seguinte linha:

```
0 * * * * /caminho/para/speedtest_log.sh
```

### Considerações Finais

*   O arquivo `speedtest_results.json` será criado no diretório onde você executar o script ou o alias.
*   Para visualizar os resultados, você pode usar um editor de texto ou um visualizador de JSON.
*   Para analisar os dados, você pode usar o `jq` ou importá-los para uma ferramenta de análise de dados.

**Observação:** A versão em script oferece mais flexibilidade e controle de erros do que o alias. Recomenda-se o uso do script para ambientes de produção ou se você precisar de recursos adicionais.

**Repositório do GitHub:**

*   [Insira aqui o link para o repositório do GitHub, caso você decida compartilhar o código publicamente]
