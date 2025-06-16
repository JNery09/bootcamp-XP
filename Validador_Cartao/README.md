import re

def luhn_check(card_number):
    digits = [int(d) for d in str(card_number)][::-1]
    total = 0
    for i, digit in enumerate(digits):
        if i % 2 == 1:
            doubled = digit * 2
            total += doubled - 9 if doubled > 9 else doubled
        else:
            total += digit
    return total % 10 == 0

def get_card_brand(card_number):
    card_number = str(card_number)

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
        return "Desconhecida"

def validar_tamanho(bandeira, numero):
    tamanho = len(numero)
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
    if bandeira not in regras:
        return False
    return tamanho in regras[bandeira]

def validar_cartao(cartao):
    cartao = re.sub(r"\D", "", cartao)  # Remove espaços e hífens
    bandeira = get_card_brand(cartao)

    if bandeira == "Desconhecida":
        print("Número inválido: os dígitos não correspondem a nenhuma bandeira conhecida.")
        return

    valido_tamanho = validar_tamanho(bandeira, cartao)
    valido_luhn = luhn_check(cartao)

    print(f"Cartão: {cartao}")
    print(f"Bandeira: {bandeira}")
    print(f"Tamanho válido: {'Sim' if valido_tamanho else 'Não'}")
    print(f"Cartão válido (Luhn): {'Sim' if valido_luhn else 'Não'}")

    if valido_tamanho and valido_luhn:
        print("Cartão é válido.")
    else:
        print("Cartão inválido.")

# Execução principal
if _name_ == "_main_":
    numero = input("Digite o número do cartão: ")
    validar_cartao(numero)
