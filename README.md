# pokeCatalog - Readme

## 📌 Visão Geral

O **pokeCatalog** é uma **aplicação web** de apoio para jogadores que estão iniciando uma *run* em qualquer jogo da franquia Pokémon. O objetivo principal é **facilitar a montagem de times**, integrando dados da [PokeAPI](https://pokeapi.co/) e filtrando essas informações de acordo com o **jogo específico** que o usuário está jogando.

O usuário acessa o site, se **cadastra/faz login** (usuário e senha) e, a partir daí, pode criar, salvar e consultar **catálogos** vinculados à sua conta — podendo ter múltiplos catálogos para diferentes runs/jogos, acessíveis de qualquer dispositivo.

A ideia central é resolver um problema comum: nem todo Pokémon, ataque, forma ou evolução está disponível em todas as versões do jogo. O pokeCatalog cruza os dados brutos da API com as **regras de disponibilidade de cada jogo/geração** para apresentar apenas o que é realmente utilizável naquela run.

### Exemplos do problema que o projeto resolve
- Em **Pokémon FireRed/LeafGreen**, não existem Pokémon da 3ª geração em diante — esses devem ser descartados do menu de seleção.
- Em **Pokémon X**, não é possível obter um **Charizard Mega Y** (exclusivo da versão Y) — variações exclusivas de versão também devem ser filtradas.
- Ataques aprendidos por *level up*, *TM/HM*, *tutor* ou *breeding* podem variar entre versões — o catálogo deve indicar a forma de obtenção correta para o jogo selecionado.
- Locais de captura (rotas, cavernas, eventos, trocas) também mudam entre versões e precisam ser filtrados corretamente.

---

## 🎯 Objetivo do Produto

Permitir que o jogador, autenticado no site, escolha o jogo (e versão) que vai jogar e, a partir disso, receba:
1. Uma lista de Pokémon **realmente disponíveis** naquele jogo.
2. Para cada Pokémon, **onde encontrá-lo** (rota, método, condição).
3. **Quais ataques** ele pode aprender, **em que nível** ou **por qual método** (level, TM, evento, tutor).
4. Suporte à montagem de um time coerente com as limitações da run escolhida.
5. A possibilidade de **salvar esse catálogo/time na sua conta** e acessá-lo depois, em qualquer dispositivo.

---

## ⚙️ Requisitos Funcionais (RF)

| ID | Requisito |
|------|-----------|
| RF01 | O sistema deve permitir cadastro e login (usuário/e-mail + senha). |
| RF02 | O sistema deve permitir que o usuário selecione um jogo/versão específico (ex: FireRed, X, Platinum). |
| RF03 | O sistema deve consumir a PokeAPI e filtrar Pokémon, formas e ataques de acordo com a versão selecionada, descartando o que não existe/não é obtível naquele jogo. |
| RF04 | O sistema deve exibir, para cada Pokémon disponível, onde capturá-lo e quais ataques ele pode aprender (nível/método de obtenção). |
| RF05 | O sistema deve permitir a montagem de um time (até 6 Pokémon) com base nos Pokémon filtrados. |
| RF06 | O sistema deve permitir salvar, listar, editar e excluir catálogos vinculados à conta do usuário. |

---

## 🛡️ Requisitos Não Funcionais (RNF)

| ID | Requisito |
|------|-----------|
| RNF01 | **Segurança**: senhas armazenadas com hash e comunicação via HTTPS. |
| RNF02 | **Isolamento de dados**: um usuário só pode acessar seus próprios catálogos. |
| RNF03 | **Desempenho**: respostas da PokeAPI devem ser cacheadas para reduzir latência. |
| RNF04 | **Manutenibilidade**: regras de disponibilidade por jogo/versão centralizadas em configuração, separadas do código de negócio. |
| RNF05 | **Usabilidade**: a interface deve ser simples e responsiva (desktop e mobile).

---

## 🧩 Escopo Inicial (MVP)

- [ ] Cadastro e login de usuário (usuário/e-mail + senha).
- [ ] Seleção de jogo/versão (começar com poucas versões: ex. FireRed, Platinum, X/Y).
- [ ] Mapeamento manual/config de Pokédex regional por versão.
- [ ] Consulta e cache de dados da PokeAPI (Pokémon, moves, locations).
- [ ] Filtro de Pokémon disponíveis por versão.
- [ ] Exibição de locais de captura.
- [ ] Exibição de movepool filtrado por versão + método de obtenção.
- [ ] Montagem simples de time (seleção de até 6 Pokémon).
- [ ] Salvar catálogo/time na conta do usuário.
- [ ] Área "Meus Catálogos" com listagem, edição e exclusão.

## 🔭 Fora do escopo inicial (futuro)
- Sugestão automática de time balanceado (cobertura de tipos).
- Cálculo de efetividade de tipo entre o time montado e os líderes de ginásio.
- Login social (Google/Discord) ou autenticação via terceiros.
- Compartilhamento público de catálogos entre usuários.
- Aplicativo mobile nativo.
- Suporte a nicknames, shiny tracking ou dados de competitivo (natureza, IV/EV).

---

## 🔗 Fontes de Dados

- [PokeAPI](https://pokeapi.co/) — dados de Pokémon, moves, species, locations, generations e versions.
- Necessário complementar com regras de exclusividade de versão que a API não expõe de forma direta (ex: Mega evoluções exclusivas, Pokémon exclusivos de versão), possivelmente via arquivo de configuração mantido manualmente.

---

## 📂 Estrutura sugerida (a evoluir)

```
pokeCatalog/
├── backend/
│   ├── config/
│   │   └── versions/        # regras de disponibilidade por jogo/versão
│   ├── src/
│   │   ├── auth/             # cadastro, login, sessão/token
│   │   ├── api/               # camada de integração com a PokeAPI
│   │   ├── filters/           # lógica de filtragem por versão/geração
│   │   ├── catalog/           # regras e persistência dos catálogos do usuário
│   │   └── team/               # lógica de montagem de time
│   ├── db/                    # models/migrations do banco de dados
│   └── cache/                 # cache de respostas da PokeAPI
├── frontend/
│   ├── src/
│   │   ├── pages/              # login, cadastro, catálogo, meus catálogos
│   │   ├── components/         # componentes reutilizáveis de UI
│   │   └── services/            # chamadas à API do backend
│   └── public/
└── pokeCatalog - Readme.md
```

### 🏗️ Visão de Arquitetura (alto nível)

- **Frontend (web)**: interface onde o usuário se cadastra/loga, escolhe o jogo/versão, monta e visualiza catálogos.
- **Backend/API interna**: responsável por autenticação, regras de filtragem por versão, integração com a PokeAPI e persistência dos catálogos.
- **Banco de dados**: armazena usuários (credenciais) e catálogos/times salvos, vinculados a cada usuário.
- **PokeAPI**: fonte externa de dados de Pokémon, moves, locations e species, consumida pelo backend.

---

## 📝 Notas

Este documento é o ponto de partida do projeto. Os requisitos acima devem ser revisados conforme o desenvolvimento avança e novos casos de exclusividade entre versões forem mapeados.
