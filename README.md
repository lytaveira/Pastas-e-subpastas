# Pastas-e-subpastas
# Um código da linguagem python para criar pastas e subpastas sem gastar tempo.

import os
import tkinter as tk
from tkinter import messagebox, simpledialog

class CriadorDePastasApp:
    def __init__(self, root):
        self.root = root
        self.root.title("Criador de Pastas")

        # Elementos da interface
        self.label_pasta_principal = tk.Label(root, text="Nome da Pasta Principal:")
        self.entry_pasta_principal = tk.Entry(root)
        self.label_quantidade_subpastas = tk.Label(root, text="Quantidade de Subpastas:")
        self.entry_quantidade_subpastas = tk.Entry(root)
        self.button_executar = tk.Button(root, text="Executar", command=self.criar_pastas)

        # Layout da interface
        self.label_pasta_principal.grid(row=0, column=0, padx=10, pady=5, sticky=tk.W)
        self.entry_pasta_principal.grid(row=0, column=1, padx=10, pady=5)
        self.label_quantidade_subpastas.grid(row=1, column=0, padx=10, pady=5, sticky=tk.W)
        self.entry_quantidade_subpastas.grid(row=1, column=1, padx=10, pady=5)
        self.button_executar.grid(row=2, column=0, columnspan=2, pady=10)

    def criar_pastas(self):
        # Solicitar nome da pasta principal
        nome_pasta_principal = self.entry_pasta_principal.get()

        # Solicitar quantidade de subpastas
        quantidade_subpastas = int(self.entry_quantidade_subpastas.get())

        # Criar a pasta principal
        caminho_pasta_principal = os.path.join(os.path.expanduser('~'), 'Desktop', nome_pasta_principal)
        os.makedirs(caminho_pasta_principal, exist_ok=True)
        messagebox.showinfo("Sucesso", f"Pasta principal '{nome_pasta_principal}' criada com sucesso!")

        # Criar as subpastas
        for i in range(1, quantidade_subpastas + 1):
            self.criar_subpastas(caminho_pasta_principal, i)

    def criar_subpastas(self, caminho_pai, num_subpasta):
        nome_subpasta = self.solicitar_nome(f"Digite o nome da Subpasta {num_subpasta} em '{os.path.basename(caminho_pai)}': ")
        caminho_subpasta = os.path.join(caminho_pai, nome_subpasta)
        os.makedirs(caminho_subpasta, exist_ok=True)
        messagebox.showinfo("Sucesso", f"Subpasta '{nome_subpasta}' criada com sucesso!")

        # Opção para criar mais pastas dentro da subpasta
        while True:
            criar_mais = messagebox.askyesno("Criar mais pastas?", f"Deseja criar mais pastas dentro de '{nome_subpasta}'?")
            if criar_mais:
                self.criar_subpastas(caminho_subpasta, num_subpasta + 1)
            else:
                break

    def solicitar_nome(self, mensagem):
        nome = simpledialog.askstring("Digite o Nome", mensagem)
        return nome

# Iniciar a aplicação
root = tk.Tk()
app = CriadorDePastasApp(root)
root.mainloop()
