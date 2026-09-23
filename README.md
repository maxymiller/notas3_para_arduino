# notas3_para_arduino [BETA]

Pega as notas3 criadas pelo `wav_para_hz` e envia para o Arduino.

Projeto desenvolvido para transformar as informações de frequência geradas pelo `wav_para_hz` em uma sequência de notas que pode ser carregada e executada pelo Arduino.

Com ajuda do ChatGPT para aprender e desenvolver o código.

---

# Executa

## 1. Conectar ao Arduino

Execute:

```bash
./arduino_input.sh <arduino>
```

Onde `<arduino>` é a porta serial do Arduino.

Exemplo:

```bash
./arduino_input.sh /dev/ttyACM0
```

O programa utiliza o `picocom` para comunicação com o Arduino.

Para sair do `picocom`:

```text
Ctrl+A
Ctrl+X
```

---

## 2. Enviar um arquivo de notas3

Execute:

```bash
./run.sh <notas3>
```

Exemplo:

```bash
./run.sh musica-notas3.txt
```

O `run.sh` inicia a comunicação e utiliza o `main.sh` para enviar o arquivo de notas ao Arduino.

---

## 🌐 Tinkercad

Código e circuito do Arduino:

👉 **[Abrir o projeto no Tinkercad](https://www.tinkercad.com/things/8KXw74EyEOt-notas3paraarduino)**

---

# Serial Monitor [SHELL]

O Arduino possui um pequeno shell pela porta serial para receber comandos e programas de notas.

A comunicação utiliza **115200 baud**.

---

## NEW

```text
NEW
```

Cria uma nova lista de notas.

Ao executar `NEW`, o programa:

- limpa as notas carregadas anteriormente;
- zera a quantidade de notas;
- limpa os metadados do programa.

---

## LIST

```text
LIST
```

Lista o programa atualmente carregado no Arduino.

Mostra:

- nome do programa;
- tempo de cada bloco;
- `top_n`;
- frequência máxima;
- capacidade reservada para as notas;
- notas no formato `frequencia quantidade`.

Exemplo:

```text
# nome=a world I build for you
# tempo_bloco=0.050000
# top_n=5
# awk_hz_max=1200.00
# arduino_tamanho=100

0 197
710 1
1180 1
940 1
470 1
710 1
```

### Formato das notas

Cada linha possui:

```text
frequencia quantidade
```

Por exemplo:

```text
710 1
```

significa uma nota de **710 Hz** com duração de **1 bloco**.

Uma frequência `0` representa silêncio.

---

## RUN

```text
RUN
```

Executa as notas carregadas.

Durante a execução:

- as frequências são reproduzidas pelo speaker;
- a duração é calculada usando `tempo_bloco`;
- `quantidade` determina quantos blocos a nota permanece tocando;
- ao terminar, o Arduino retorna para `READY`.

Exemplo:

```text
# tempo_bloco=0.050000
710 1
```

Nesse caso, `710 Hz` será reproduzido durante aproximadamente **50 ms**.

---

# Formato do arquivo de notas3

O programa recebe arquivos contendo metadados e notas.

Exemplo:

```text
# nome=a world I build for you
# tempo_bloco=0.050000
# top_n=5
# awk_hz_max=1200.00

0 197
710 1
1180 1
940 1
470 1
710 1
```

## Metadados

| Campo | Descrição |
|---|---|
| `nome` | Nome do programa/música |
| `tempo_bloco` | Duração de cada bloco em segundos |
| `top_n` | Informação do processamento realizado pelo `wav_para_hz` |
| `awk_hz_max` | Informação do processamento realizado pelo `wav_para_hz` |

> `top_n` e `awk_hz_max` são mantidos apenas como informações sobre como o `wav_para_hz` criou as notas3. O Arduino não utiliza esses valores para executar as notas.

---

# Protocolo

O Arduino responde aos comandos com `READY` quando está pronto para receber o próximo comando.

Fluxo básico:

```text
NEW
READY

# nome=...
READY

# tempo_bloco=...
READY

710 1
READY

1180 1
READY

RUN
READY
```

Em caso de erro:

```text
ERROR
```

---

# Estrutura do projeto

O código possui algumas partes principais:

- **Shell** — recebe e interpreta os comandos pela serial.
- **Programa de notas** — armazena frequência e quantidade.
- **Metadados** — guarda informações do arquivo de notas.
- **Tela** — controla o LCD 16x2.
- **Speaker** — reproduz as frequências.
- **RUN** — executa a sequência de notas.

---

# Hardware

O projeto foi testado com um circuito de Arduino no Tinkercad utilizando:

- Arduino;
- LCD 16x2;
- speaker/piezo;
- conexões digitais para o LCD.

O código utiliza a biblioteca `LiquidCrystal`.

---

# Status

**[BETA]**

O projeto ainda está em desenvolvimento.

Novos comandos, melhorias no protocolo serial, reprodução de áudio e recursos para o LCD podem ser adicionados futuramente.

---

## 🤖 Desenvolvimento

Projeto desenvolvido com auxílio do **ChatGPT**, principalmente para aprendizado de:

- C/C++ para Arduino;
- comunicação serial;
- estruturas e ponteiros;
- gerenciamento dinâmico de memória;
- classes em C++;
- LCD 16x2;
- reprodução de frequências;
- integração entre Bash, arquivos de notas e Arduino.
