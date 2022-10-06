import menu
import pymysql.cursors

class adm():
    def __init__(self):
        pass
    def conexao(self):
        try:
            self.dado = pymysql.connect(
                host='localhost',
                user='root',
                password='',
                database='joias',
                charset='utf8mb4',
                cursorclass=pymysql.cursors.DictCursor
            )
        except:
            print('Ops, algo deu errado')

    def cadastro(self):
        self.conexao()
        tipo = str(input('Tipo: '))
        banho = str(input('Banho: '))
        quantidade = int(input('Quantidade: '))
        cliente = str(input('Cleinte: '))
        valor = int(input('Valor: '))
        pago = str(input('Pago: '))
        dados = [tipo, banho, quantidade, cliente, valor, pago]
        nova_joia().registrar(dados)

    def total_joais(self):
        self.conexao()

        with self.dado.cursor() as curso:
            curso.execute('select * from joias')
            joias = curso.fetchall()
            print(joias)


    def atualizar(self):
        self.conexao()

        idjoia = int(input('Qual o id do produto? '))

        try:
            with self.dado.cursor() as curso:
                curso.execute(f'SELECT * FROM  joias WHERE idjoia = {idjoia}')
                joias = curso.fetchall()

        except:
            print("Algo deu errado, verifique novamente se os dados estão corretos")

            tipo = str(input('Tipo: '))
            banho = str(input('Banho: '))
            quantidade = int(input('Quantidade: '))
            cliente = str(input('Cleinte: '))
            valor = int(input('Valor: '))
            pago = int(input('Pago: '))

            with self.dado.cursor() as curso:
                curso.execute(f'UPDATE joias SET tipo = {tipo}, banho = {banho}, quantidade = {quantidade}, cliente = {cliente}, valor = {valor}, pago = {pago}')
                self.dado.commit()
                print('Dados alterados')

    def excluir(self):
        self.conexao()

        idjoia = int(input('Qual é o id da joia que você desja deletar? '))
        with self.dado.cursor() as curso:
            curso.execute(f'DELETW FROM joias WHERE idjoia = {idjoia}')
            self.dado.commit()
            print('Produto deletado')

class nova_joia(adm):
    def __init__(self):
        pass

    def registrar(self, dados):
        self.conexao()

        with self.dado.cursor() as curso:
            sql = 'INSERT INTO joias (tipo, banho, quantidade, cliente, valor, pago) values (%s, %s, %s, %s, %s, %s)'
            curso.execute(sql, dados)
            self.dados.commit()
            print('Joias Cadastradas')
            
            
import menuprincipal as mp

def main():
    print("""
    1- Cadastrar Joia
    2- Atualizar Dados
    3- Deletar Joia
    4- Mostrar Joias
    """)

    while True:
        opcao = int(input('Qual sua opção? '))

        if opcao == 1:
            mp.adm().cadastro()

        elif opcao == 2:
            mp.adm().atualizar()

        elif opcao == 3:
            mp.adm().atualizar()

        elif opcao == 4:
            mp.adm().total_joais()

        else:
            print('Opção não encontrada')
if __name__ == '__main__':
    main()

            
