# Script de validação de carão de credito informando se o cartão é valido e qual a bandeira ele pertence
# Script desenvolvido por IA com a linguagem em Python



import re  # Importa o módulo de expressões regulares para validação dos padrões dos cartões

# Função para verificar se o número do cartão é válido usando o algoritmo de Luhn
def luhn_check(card_number):
    # Converte os dígitos do número do cartão para inteiros e inverte a ordem
    digits = [int(d) for d in str(card_number)][::-1]
    total = 0

    # Aplica o algoritmo de Luhn
    for i, digit in enumerate(digits):
        if i % 2 == 1:  # A cada segundo dígito, começando do fim
            doubled = digit * 2
            # Se o valor dobrado for maior que 9, subtrai 9
            total += doubled - 9 if doubled > 9 else doubled
        else:
            total += digit
    # O número é válido se o total for divisível por 10
    return total % 10 == 0

# Função para identificar a bandeira do cartão com base em padrões conhecidos
def get_card_brand(card_number):
    card_number = str(card_number)

    # Cada expressão regular abaixo identifica um tipo de bandeira específica
    if re.match(r"^4\d{12}(\d{3})?(\d{3})?$", card_number):
        return "Visa"
    elif re.match(r"^(5[1-5]\d{14}|2(2[2-9][1-9]|2[3-9]\d|[3-6]\d{2}|7([01]\d|20))\d{12})$", card_number):
        return "Mastercard"
    elif re.match(r"^3[47]\d{13}$", card_number):
        return "American Express"
    elif re.match(r"^6(011|5\d{2}|4[4-9]\d)\d{12}$", card_number):
        return "Discover"
    elif re.match(r"^(4011(78|79)|438935|4576(31|32)|504175|627780|636297|636368|650\d{3}|651\d{3}|655\d{3})\d*$", card_number):
        return "Elo"
    elif re.match(r"^3(0[0-5]|[68]\d)\d{11}$", card_number):
        return "Diners Club"
    elif re.match(r"^35\d{14}$", card_number):
        return "JCB"
    elif re.match(r"^2(014|149)\d{11}$", card_number):
        return "enRoute"
    elif re.match(r"^(50|5[6-9]|6[0-9])\d{10,17}$", card_number):
        return "Maestro"
    elif re.match(r"^(6334|6767)\d{12,15}$", card_number):
        return "Solo"
    elif re.match(r"^(4903|4905|4911|4936|564182|633110|6333|6759)\d{6,10}$", card_number):
        return "Switch"
    elif re.match(r"^(6304|6706|6771|6709)\d{12,15}$", card_number):
        return "Laser"
    elif re.match(r"^8699\d{11}$", card_number):
        return "Voyager"
    elif re.match(r"^(3841|60\d{2}|637[0-5]\d{2})\d{10,12}$", card_number):
        return "Hipercard"
    elif re.match(r"^50\d{14}$", card_number):
        return "Aura"
    else:
        return "Desconhecida"  # Retorna como desconhecida se não corresponder a nenhum padrão

# Função para validar se o número do cartão tem o tamanho correto conforme sua bandeira
def validar_tamanho(bandeira, numero):
    tamanho = len(numero)

    # Regras com os comprimentos válidos para cada bandeira
    regras = {
        "Visa": [13, 16, 19],
        "Mastercard": [16],
        "Elo": [16],
        "Hipercard": [13, 16, 19],
        "American Express": [15],
        "Diners Club": [14, 16],
        "Discover": [16],
        "JCB": [16],
        "Maestro": list(range(12, 20)),
        "enRoute": [15],
        "Voyager": [15],
        "Aura": [16],
        "Solo": [16, 18, 19],
        "Switch": [16, 18, 19],
        "Laser": [16, 18, 19],
    }

    # Retorna False se a bandeira não estiver no dicionário
    if bandeira not in regras:
        return False

    # Verifica se o tamanho do número está dentro dos tamanhos válidos
    return tamanho in regras[bandeira]

# Função principal para validar o cartão
def validar_cartao(cartao):
    cartao = re.sub(r"\D", "", cartao)  # Remove todos os caracteres que não são dígitos

    bandeira = get_card_brand(cartao)  # Identifica a bandeira do cartão

    if bandeira == "Desconhecida":
        print("Número inválido: os dígitos não correspondem a nenhuma bandeira conhecida.")
        return

    valido_tamanho = validar_tamanho(bandeira, cartao)  # Verifica tamanho
    valido_luhn = luhn_check(cartao)  # Verifica validade pelo algoritmo de Luhn

    # Exibe os resultados da validação
    print(f"Cartão: {cartao}")
    print(f"Bandeira: {bandeira}")
    print(f"Tamanho válido: {'Sim' if valido_tamanho else 'Não'}")
    print(f"Cartão válido (Luhn): {'Sim' if valido_luhn else 'Não'}")

    if valido_tamanho and valido_luhn:
        print("Cartão é válido.")
    else:
        print("Cartão inválido.")

# Ponto de entrada do script
if __name__ == "__main__":  # Corrigido de "_name_" para "__name__"
    numero = input("Digite o número do cartão: ")
    validar_cartao(numero)
