# RUST 

## O que é 

O **Rust** nasceu para resolver uma dor clássica da programação: ter a velocidade bruta do C/C++, mas sem a dor de cabeça constante com falhas de segurança e vazamentos de memória. Criado pela Mozilla, ele entrega controle total sobre o hardware sem deixar o código vulnerável.

O grande truque está no compilador. Enquanto outras linguagens usam um "coletor de lixo" pesado em segundo plano ou jogam a responsabilidade da memória nas costas do dev, o Rust checa todas as permissões de acesso antes mesmo de gerar o executável. Se tiver risco de quebrar ou dar conflito entre threads, o código simplesmente não compila.

Na prática, isso gerou uma linguagem rápida, previsível e incrivelmente estável. Não à toa, virou a queridinha da indústria para construir ferramentas de linha de comando, serviços em nuvem de alta demanda e até partes vitais do próprio kernel do Linux.



## Instalação

No Linux - Terminal:

```
# 1. Instalar dependências básicas
sudo apt update && sudo apt install -y curl build-essential

# 2. Instalar o Rust via script oficial
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

# 3. Ativar o Rust no terminal atual
source "$HOME/.cargo/env"

# 4. Verificar se funcionou
rustc --version
cargo --version

```


## O que é CARGO

O **Cargo** é o gerenciador de pacotes e o sistema de automação de compilação (*build system*) oficial da linguagem **Rust**.

Ele resolve desde o download de bibliotecas de terceiros até a compilação e teste do código, atuando de forma equivalente ao `npm` no Node.js, `pip` no Python ou `composer` no PHP — mas com integração nativa ao compilador `rustc`.

---

**Principais Funções**

* **Gerenciamento de Dependências:** Baixa e compila bibliotecas (chamadas de *crates* no ecossistema Rust) diretamente do repositório oficial [crates.io](https://crates.io).
* **Compilação e Execução:** Coordena a compilação do seu código com as flags corretas sem que você precise chamar o `rustc` manualmente.
* **Estruturação de Projetos:** Cria uma estrutura de pastas padronizada com um único comando.
* **Testes e Benchmarks:** Executa baterias de testes unitários e de integração integradas à linguagem.

---

**Arquivos de Controle**

* **`Cargo.toml`:** O manifesto do projeto. É onde você declara metadados (nome, versão, autor) e as dependências que o código utiliza.
* **`Cargo.lock`:** Um arquivo gerado automaticamente com o hash e a versão exata de cada dependência compilada, garantindo builds reproduzíveis em qualquer máquina.

---

**Comandos mais Usados**

| Comando | O que faz |
| --- | --- |
| `cargo new nome_projeto` | Cria um novo projeto estruturado em Rust - início de tudo |
| `cargo build` | Compila o código e gera o executável |
| `cargo run` | Compila e executa o programa em um único passo |
| `cargo check` | Verifica erros de sintaxe e tipos rapidamente (sem gerar binário) |
| `cargo test` | Roda a suíte de testes do projeto |
| `cargo add nome_crate` | Adiciona uma dependência ao `Cargo.toml` |



## Estrutura das pastas num projeto inicial simples

Essa é a estrutura padrão gerada e gerenciada pelo **Cargo**. Cada item tem um papel bem definido no ciclo de desenvolvimento e compilação:

---

### `Cargo.toml`

É o **manifesto do projeto** (escrito no formato TOML). É o arquivo de configuração que você edita diretamente.

* **Metadados:** Define o nome do pacote, versão, autores e a edição do Rust (ex: `2021`).
* **Dependências:** Onde você declara as bibliotecas externas (*crates*) que seu projeto usa (ex: `serde = "1.0"`).

### `Cargo.lock`

É o arquivo de **travamento exato de versões**.

* Gerado e atualizado automaticamente pelo Cargo.
* Registra a versão exata e o hash criptográfico de cada biblioteca e sub-dependência baixada.
* **Função:** Garante compilações 100% reproduzíveis — qualquer pessoa (ou servidor CI/CD) que compilar o projeto usará rigorosamente as mesmas versões que você usou. *(Você quase nunca edita este arquivo manualmente).*

### `src/`

É o diretório do seu **código-fonte** (*source code*).

* Para uma aplicação executável (binário), o ponto de entrada padrão é o arquivo **`src/main.rs`** (onde fica a função `fn main()`).
* Se fosse uma biblioteca, o arquivo principal seria `src/lib.rs`.
* Novos módulos e arquivos `.rs` criados por você devem ficar dentro desta pasta.

### `target/`

É a pasta de **artefatos de compilação**.

* Criada automaticamente quando você roda `cargo build`, `cargo run` ou `cargo check`.
* Contém os binários finais compilados (geralmente em `target/debug/` para desenvolvimento ou `target/release/` para produção otimizada), arquivos intermediários e dependências já compiladas para agilizar recompilações.
* **Dica:** Esta pasta costuma ficar pesada e **nunca deve ser versionada no Git** (o `cargo new` já adiciona `target/` no `.gitignore` por padrão). Para limpá-la e liberar espaço em disco, basta rodar:
```bash
cargo clean

```

---

| Item | Tipo | Você edita? | Finalidade |
| --- | --- | --- | --- |
| `Cargo.toml` | Arquivo | **Sim** | Configurações do projeto e lista de dependências |
| `Cargo.lock` | Arquivo | Não | Garante versões exatas para reprodutibilidade |
| `src/` | Pasta | **Sim** | Seu código Rust (`main.rs`, módulos, etc.) |
| `target/` | Pasta | Não | Binários gerados e cache de compilação |


```sh
treinamento_rust/
├── Cargo.toml          <- Manifesto (você edita: metadados e dependências)
├── Cargo.lock          <- Trava de versões exatas das bibliotecas (automático)
│
├── src/                <- Código-fonte (onde você programa)
│   ├── main.rs         <- Ponto de entrada do programa (fn main)
│   └── utils.rs        <- Módulos/funções extras criados por você (opcional)
│
└── target/             <- Pasta de compilação (gerada pelo Cargo / ignorar no Git)
    ├── CACHEDIR.TAG    <- Tag de cache do Cargo
    │
    ├── debug/          <- Gerado por: cargo build / cargo run
    │   ├── treinamento_rust   <- BINÁRIO executável (com símbolos de debug)
    │   ├── deps/              <- Dependências compiladas em modo debug
    │   └── incremental/       <- Cache para recompilação incremental rápida
    │
    └── release/        <- Gerado por: cargo build --release
        ├── treinamento_rust   <- BINÁRIO executável FINAL (super otimizado e menor)
        └── deps/              <- Dependências compiladas com otimização máxima
```

