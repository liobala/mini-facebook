A# Mini Facebook em Python

Sistema simples que simula funcionalidades basicas de uma rede social:

- cadastro e remocao de usuarios;
- criacao e remocao de amizades bidirecionais;
- listagem de amigos de um usuario;
- listagem de todos os perfis;
- armazenamento persistente em arquivo JSON.

## Como executar

```bash
python mini_facebook.py
```

Ao iniciar, o programa tenta carregar os dados do arquivo `dados_rede.json`.
Ao sair pela opcao `0`, os dados sao salvos automaticamente.

## Estrutura de dados

O programa usa a classe `Usuario` para representar cada perfil, com:

- `id`: identificador unico;
- `nome`: nome completo;
- `idade`: idade do usuario;
- `amigos`: conjunto com os IDs dos amigos.

A classe `RedeSocial` guarda todos os usuarios em um dicionario, usando o ID como chave.
Isso facilita a busca por usuario, evita IDs duplicados e deixa a criacao/remocao de amizades mais simples.

## Principais funcoes

- `adicionar_usuario`: cadastra um novo usuario e impede IDs repetidos.
- `remover_usuario`: remove o usuario e tambem apaga suas amizades nos outros perfis.
- `criar_amizade`: cria amizade bidirecional, impedindo duplicidade e amizade consigo mesmo.
- `remover_amizade`: remove o vinculo dos dois usuarios.
- `listar_amizades`: mostra ID e nome dos amigos de um usuario.
- `listar_perfis`: mostra ID, nome, idade e quantidade de amigos de cada perfil.
- `salvar` e `carregar`: gravam e recuperam os dados usando JSON.
