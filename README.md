# controle-de-vendas
import os
from datetime import datetime
from openpyxl import Workbook

ARQUIVO = "vendas.txt"
ARQUIVO_EXCEL = "vendas.xlsx"


def cadastrar_venda():
    cliente = input("Nome do cliente: ")
    produto = input("Produto: ")
    valor = float(input("Valor da venda: R$ "))

    data = datetime.now().strftime("%d/%m/%Y %H:%M")

    with open(ARQUIVO, "a") as f:
        f.write(f"{data};{cliente};{produto};{valor}\n")

    print("✅ Venda cadastrada com sucesso!\n")


def ver_vendas():
    if not os.path.exists(ARQUIVO):
        print("Nenhuma venda registrada.\n")
        return

    total = 0

    with open(ARQUIVO, "r") as f:
        print("\n📋 Histórico de Vendas:\n")
        for linha in f:
            data, cliente, produto, valor = linha.strip().split(";")
            print(f"Data: {data}")
            print(f"Cliente: {cliente}")
            print(f"Produto: {produto}")
            print(f"Valor: R$ {valor}")
            print("-" * 30)

            total += float(valor)

    print(f"💰 Total vendido: R$ {total:.2f}\n")


def exportar_excel():
    if not os.path.exists(ARQUIVO):
        print("Nenhuma venda para exportar.\n")
        return

    wb = Workbook()
    ws = wb.active
    ws.title = "Relatório de Vendas"

    # Cabeçalho
    ws.append(["Data", "Cliente", "Produto", "Valor"])

    total = 0

    with open(ARQUIVO, "r") as f:
        for linha in f:
            data, cliente, produto, valor = linha.strip().split(";")
            ws.append([data, cliente, produto, float(valor)])
            total += float(valor)

    ws.append(["", "", "TOTAL", total])

    wb.save(ARQUIVO_EXCEL)

    print(f"📊 Arquivo Excel '{ARQUIVO_EXCEL}' criado com sucesso!\n")


def menu():
    while True:
        print("==== SISTEMA DE VENDAS ====")
        print("1 - Cadastrar venda")
        print("2 - Ver vendas")
        print("3 - Exportar para Excel")
        print("4 - Sair")

        opcao = input("Escolha uma opção: ")

        if opcao == "1":
            cadastrar_venda()
        elif opcao == "2":
            ver_vendas()
        elif opcao == "3":
            exportar_excel()
        elif opcao == "4":
            print("Encerrando sistema...")
            break
        else:
            print("Opção inválida!\n")


if __name__ == "__main__":
    menu()

