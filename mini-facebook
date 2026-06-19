import json
from dataclasses import dataclass, field
from pathlib import Path


ARQUIVO_DADOS = Path("dados_rede.json")


@dataclass
class Usuario:
    id: int
    nome: str
    idade: int
    amigos: set[int] = field(default_factory=set)


class RedeSocial:
    def __init__(self) -> None:
        self.usuarios: dict[int, Usuario] = {}

    def adicionar_usuario(self, id_usuario: int, nome: str, idade: int) -> bool:
        if id_usuario in self.usuarios:
            print("Erro: ja existe um usuario com esse ID.")
            return False

        self.usuarios[id_usuario] = Usuario(id_usuario, nome, idade)
        print("Usuario cadastrado com sucesso.")
        return True

    def remover_usuario(self, id_usuario: int) -> bool:
        usuario = self.usuarios.get(id_usuario)
        if usuario is None:
            print("Erro: usuario nao encontrado.")
            return False

        for id_amigo in list(usuario.amigos):
            self.usuarios[id_amigo].amigos.discard(id_usuario)

        del self.usuarios[id_usuario]
        print("Usuario removido com sucesso.")
        return True

    def criar_amizade(self, id_a: int, id_b: int) -> bool:
        if id_a == id_b:
            print("Erro: um usuario nao pode ser amigo de si mesmo.")
            return False

        usuario_a = self.usuarios.get(id_a)
        usuario_b = self.usuarios.get(id_b)

        if usuario_a is None or usuario_b is None:
            print("Erro: um ou ambos os usuarios nao foram encontrados.")
            return False

        if id_b in usuario_a.amigos:
            print("Erro: amizade ja cadastrada.")
            return False

        usuario_a.amigos.add(id_b)
        usuario_b.amigos.add(id_a)
        print("Amizade criada com sucesso.")
        return True

    def remover_amizade(self, id_a: int, id_b: int) -> bool:
        usuario_a = self.usuarios.get(id_a)
        usuario_b = self.usuarios.get(id_b)

        if usuario_a is None or usuario_b is None:
            print("Erro: um ou ambos os usuarios nao foram encontrados.")
            return False

        if id_b not in usuario_a.amigos:
            print("Erro: amizade nao existe.")
            return False

        usuario_a.amigos.remove(id_b)
        usuario_b.amigos.remove(id_a)
        print("Amizade removida com sucesso.")
        return True

    def listar_amizades(self, id_usuario: int) -> None:
        usuario = self.usuarios.get(id_usuario)
        if usuario is None:
            print("Erro: usuario nao encontrado.")
            return

        print(f"\nAmigos de {usuario.nome} (ID {usuario.id}):")
        if not usuario.amigos:
            print("Nenhuma amizade cadastrada.")
            return

        for id_amigo in sorted(usuario.amigos):
            amigo = self.usuarios[id_amigo]
            print(f"- ID {amigo.id}: {amigo.nome}")

    def listar_perfis(self) -> None:
        print("\nPerfis cadastrados:")
        if not self.usuarios:
            print("Nenhum usuario cadastrado.")
            return

        for usuario in sorted(self.usuarios.values(), key=lambda u: u.id):
            total_amigos = len(usuario.amigos)
            print(
                f"ID: {usuario.id} | Nome: {usuario.nome} | "
                f"Idade: {usuario.idade} | Amigos: {total_amigos}"
            )

    def salvar(self, caminho: Path = ARQUIVO_DADOS) -> None:
        dados = [
            {
                "id": usuario.id,
                "nome": usuario.nome,
                "idade": usuario.idade,
                "amigos": sorted(usuario.amigos),
            }
            for usuario in self.usuarios.values()
        ]

        caminho.write_text(json.dumps(dados, indent=2, ensure_ascii=False), encoding="utf-8")

    def carregar(self, caminho: Path = ARQUIVO_DADOS) -> None:
        if not caminho.exists():
            return

        dados = json.loads(caminho.read_text(encoding="utf-8"))
        self.usuarios.clear()

        for item in dados:
            self.usuarios[item["id"]] = Usuario(
                id=item["id"],
                nome=item["nome"],
                idade=item["idade"],
                amigos=set(item.get("amigos", [])),
            )


def ler_inteiro(mensagem: str) -> int:
    while True:
        try:
            return int(input(mensagem))
        except ValueError:
            print("Entrada invalida. Digite um numero inteiro.")


def ler_nome() -> str:
    while True:
        nome = input("Nome completo: ").strip()
        if nome:
            return nome
        print("O nome nao pode ficar vazio.")


def mostrar_menu() -> None:
    print("\n========== MINI FACEBOOK ==========")
    print("1. Adicionar usuario")
    print("2. Remover usuario")
    print("3. Criar amizade")
    print("4. Remover amizade")
    print("5. Listar amizades de um usuario")
    print("6. Listar todos os perfis")
    print("7. Salvar dados")
    print("0. Sair")


def main() -> None:
    rede = RedeSocial()
    rede.carregar()

    while True:
        mostrar_menu()
        opcao = input("Escolha uma opcao: ").strip()

        if opcao == "1":
            id_usuario = ler_inteiro("ID: ")
            nome = ler_nome()
            idade = ler_inteiro("Idade: ")
            rede.adicionar_usuario(id_usuario, nome, idade)

        elif opcao == "2":
            id_usuario = ler_inteiro("ID do usuario a remover: ")
            rede.remover_usuario(id_usuario)

        elif opcao == "3":
            id_a = ler_inteiro("ID do primeiro usuario: ")
            id_b = ler_inteiro("ID do segundo usuario: ")
            rede.criar_amizade(id_a, id_b)

        elif opcao == "4":
            id_a = ler_inteiro("ID do primeiro usuario: ")
            id_b = ler_inteiro("ID do segundo usuario: ")
            rede.remover_amizade(id_a, id_b)

        elif opcao == "5":
            id_usuario = ler_inteiro("ID do usuario: ")
            rede.listar_amizades(id_usuario)

        elif opcao == "6":
            rede.listar_perfis()

        elif opcao == "7":
            rede.salvar()
            print("Dados salvos com sucesso.")

        elif opcao == "0":
            rede.salvar()
            print("Dados salvos. Encerrando o sistema.")
            break

        else:
            print("Opcao invalida.")


if __name__ == "__main__":
    main()
