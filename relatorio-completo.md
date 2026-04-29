# Relatório — Atividade Prática: Sockets TCP

**Disciplina:** Sistemas Distribuídos
**Dupla:** Julia Fischer Gazolla / Fernanda Larissa Müller
**Data:** 28/04/26

---

## Nível 1 — Inspecionar o código

### 1.1 Servidor (`servidor.py`)

Descreva com suas palavras o que cada chamada abaixo faz e por que ela
é necessária:

| Chamada | O que ela faz? |
|---------|---------------|
| `socket.socket(AF_INET, SOCK_STREAM)` | | Cria um socket do tipo IPv4 e do protocolo TCP.
| `servidor_socket.bind((HOST, PORTA))` | | Associa o socket criado (host) a porta de conexão (porta).
| `servidor_socket.listen(5)` | | O socket fica em modo de escuta. O paramêtro difine quantas conexões podem estar em fila.
| `servidor_socket.accept()` | | O servidor aceita uma conexão, quando houver.
| `conn.recv(TAMANHO_BUFFER)` | | O servidor recebe dados do cliente.
| `conn.sendall(resposta.encode("utf-8"))` | | O servidor envia mensagem para o cliente.
| `conn.close()` | | O servidor encerra a comunicação com esse cliente.

### 1.2 Cliente (`cliente.py`)

| Chamada | O que ela faz? |
|---------|---------------|
| `socket.socket(AF_INET, SOCK_STREAM)` | | Cria um socket do tipo IPv4 e do protocolo TCP.
| `cliente_socket.connect((HOST_SERVIDOR, PORTA_SERVIDOR))` | | O cliente conecta ao servidor através do nome e porta.
| `cliente_socket.sendall(mensagem.encode("utf-8"))` | |  O cliente envia a mensagem codificada em bytes.
| `cliente_socket.recv(TAMANHO_BUFFER)` | | O cliente aguarda receber uma informação. O parâmetro define o tamanho máximo da mensagem.

### 1.3 Rede e contêineres

Por que o cliente usa o hostname `"servidor"` em vez de `"localhost"`
para se conectar? O que aconteceria se usasse `"localhost"`?

> _Resposta:_
O cliente usa hostname `"servidor"`, pois é onde ele quer se comunicar, que esta em ambiente externo. Se usasse `"localhost"` , estaria conectado a comunicação interna.
---

## Nível 2 — Modificar o servidor

Descreva as mudanças que você fez no `servidor.py` para:

1. Devolver a mensagem **em maiúsculas**:

   > _O que foi alterado e por quê:_

A mensagem recebida permaneceu inalterada. Antes de enviar a mensagem, foi aplicada a função upper(), disponível em Python.

2. Exibir no log o **contador de mensagens recebidas** acumulado:

   > _O que foi alterado e por quê:_

Durante o loop de aguardo por mensagens, foi adicionado o incremento da variavel "total_mensagens" e incluído na mensagem de log.

Após a modificação, você precisou rodar `docker compose up --build`.
Por que o `--build` é necessário aqui?

> _Resposta:_
O comando é necessário pois houveram modificações no código que compõe o servidor. 

while True:
        # 4. Aguardar e aceitar uma nova conexão de cliente
        conn, endereco_cliente = servidor_socket.accept()
        log(f"[SERVIDOR] Conexão estabelecida com {endereco_cliente}")

        # 5. Receber dados do cliente
        dados = conn.recv(TAMANHO_BUFFER)

        if dados:
            mensagem = dados.decode("utf-8")
            total_mensagens+=1
            log(f"[SERVIDOR] Mensagem recebida nº {total_mensagens}: '{mensagem}'")

            # 6. Devolver a mensagem ao cliente (eco)
            resposta = mensagem.upper()
            conn.sendall(resposta.encode("utf-8"))
            log(f"[SERVIDOR] Eco enviado: '{resposta}'")

        # 7. Encerrar a conexão com este cliente
        conn.close()
        log(f"[SERVIDOR] Conexão com {endereco_cliente} encerrada.")

---

## Observações livres

Anote aqui qualquer comportamento inesperado que observaram, erros que
apareceram e como resolveram, ou dúvidas que ainda têm:

>

---

## Dúvida para a próxima aula

Escreva **uma pergunta** que surgiu durante a atividade e que vocês
gostariam de ver respondida na aula seguinte:

>
