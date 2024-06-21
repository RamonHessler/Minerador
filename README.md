# Minerador
Minerador de criptomoeda
import hashlib
import time

# Classe para representar um bloco
class Bloco:
    def __init__(self, indice, timestamp, dados, hash_anterior):
        self.indice = indice
        self.timestamp = timestamp
        self.dados = dados
        self.hash_anterior = hash_anterior
        self.nonce = 0
        self.hash = self.calcular_hash()

    def calcular_hash(self):
        # Concatena os atributos do bloco e calcula o hash SHA-256
        conteudo = str(self.indice) + str(self.timestamp) + str(self.dados) + str(self.hash_anterior) + str(self.nonce)
        return hashlib.sha256(conteudo.encode()).hexdigest()

    def minerar_bloco(self, dificuldade):
        # Mineração com proof-of-work
        while self.hash[:dificuldade] != '0' * dificuldade:
            self.nonce += 1
            self.hash = self.calcular_hash()
        
        print(f"Bloco minerado: {self.hash}")

# Classe para representar a blockchain
class Blockchain:
    def __init__(self):
        self.chain = [self.criar_bloco_genesis()]
        self.dificuldade = 2  # Critério de dificuldade para o proof-of-work

    def criar_bloco_genesis(self):
        # Cria o bloco inicial (bloco genesis)
        return Bloco(0, time.time(), "Dados do bloco genesis", "0")

    def obter_ultimo_bloco(self):
        # Retorna o último bloco da blockchain
        return self.chain[-1]

    def adicionar_bloco(self, novo_bloco):
        novo_bloco.hash_anterior = self.obter_ultimo_bloco().hash
        novo_bloco.minerar_bloco(self.dificuldade)
        self.chain.append(novo_bloco)

# Testando a blockchain com alguns blocos
blockchain = Blockchain()

# Adicionando alguns blocos à blockchain
for i in range(1, 4):
    dados = f"Dados do bloco {i}"
    novo_bloco = Bloco(i, time.time(), dados, "")
    blockchain.adicionar_bloco(novo_bloco)
    print(f"Bloco {i} adicionado à blockchain\n")

# Imprimindo a blockchain
print("Blockchain:")
for bloco in blockchain.chain:
    print(f"Índice: {bloco.indice}")
    print(f"Timestamp: {bloco.timestamp}")
    print(f"Dados: {bloco.dados}")
    print(f"Hash anterior: {bloco.hash_anterior}")
    print(f"Nonce: {bloco.nonce}")
    print(f"Hash: {bloco.hash}")
    print("\n")
