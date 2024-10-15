# kriptografi
Nama  :Fathia Wardah S.Djawas

NIM  : 312210196

Kelas : TI.22.A1

import string

```
import string

# Fungsi untuk membuat matriks Playfair dari kunci
def create_playfair_matrix(key):
    key = key.replace('J', 'I')  # Menggabungkan I dan J
    key = "".join(sorted(set(key), key=key.index))  # Menghapus huruf yang berulang
    alphabet = string.ascii_uppercase.replace('J', '')  # Menghapus J dari alfabet
    matrix = key + ''.join([char for char in alphabet if char not in key])
    matrix = [list(matrix[i:i+5]) for i in range(0, 25, 5)]
    return matrix

# Fungsi untuk membagi plaintext menjadi pasangan bigram
def create_bigrams(plaintext):
    plaintext = plaintext.replace('J', 'I').replace(' ', '').upper()
    bigrams = []
    i = 0
    while i < len(plaintext):
        a = plaintext[i]
        b = plaintext[i+1] if i+1 < len(plaintext) else 'X'
        if a == b:
            bigrams.append(a + 'X')
            i += 1
        else:
            bigrams.append(a + b)
            i += 2
    if len(bigrams[-1]) == 1:
        bigrams[-1] += 'X'
    return bigrams

# Fungsi untuk mengenkripsi bigram
def encrypt_bigram(bigram, matrix):
    positions = {char: (r, c) for r, row in enumerate(matrix) for c, char in enumerate(row)}
    a, b = bigram
    ra, ca = positions[a]
    rb, cb = positions[b]

    if ra == rb:  # Jika berada pada baris yang sama, geser ke kanan
        return matrix[ra][(ca + 1) % 5] + matrix[rb][(cb + 1) % 5]
    elif ca == cb:  # Jika berada pada kolom yang sama, geser ke bawah
        return matrix[(ra + 1) % 5][ca] + matrix[(rb + 1) % 5][cb]
    else:  # Jika berada pada baris dan kolom yang berbeda, tukar posisi
        return matrix[ra][cb] + matrix[rb][ca]

# Fungsi untuk mengenkripsi Playfair Cipher
def playfair_encrypt(plaintext, key):
    matrix = create_playfair_matrix(key)
    bigrams = create_bigrams(plaintext)
    ciphertext = ''.join([encrypt_bigram(bigram, matrix) for bigram in bigrams])
    return ciphertext

# Fungsi untuk mendekripsi bigram
def decrypt_bigram(bigram, matrix):
    positions = {char: (r, c) for r, row in enumerate(matrix) for c, char in enumerate(row)}
    a, b = bigram
    ra, ca = positions[a]
    rb, cb = positions[b]

    if ra == rb:  # Jika berada pada baris yang sama, geser ke kiri
        return matrix[ra][(ca - 1) % 5] + matrix[rb][(cb - 1) % 5]
    elif ca == cb:  # Jika berada pada kolom yang sama, geser ke atas
        return matrix[(ra - 1) % 5][ca] + matrix[(rb - 1) % 5][cb]
    else:  # Jika berada pada baris dan kolom yang berbeda, tukar posisi
        return matrix[ra][cb] + matrix[rb][ca]

# Fungsi untuk mendekripsi Playfair Cipher
def playfair_decrypt(ciphertext, key):
    matrix = create_playfair_matrix(key)
    bigrams = create_bigrams(ciphertext)
    plaintext = ''.join([decrypt_bigram(bigram, matrix) for bigram in bigrams])
    return plaintext

# Data kunci dan plaintext
key = "TEKNIK INFORMATIKA"
plaintexts = [
    "GOOD BROOM SWEEP CLEAN",
    "REDWOOD NATIONAL STATE PARK",
    "JUNK FOOD AND HEALTH PROBLEMS"
]

# Enkripsi tiap plaintext
encrypted_texts = [playfair_encrypt(text, key) for text in plaintexts]

# Dekripsi tiap ciphertext
decrypted_texts = [playfair_decrypt(text, key) for text in encrypted_texts]

# Tampilkan hasil enkripsi dan dekripsi
for i, text in enumerate(plaintexts):
    print(f"Plaintext {i+1}: {text}")
    print(f"Ciphertext: {encrypted_texts[i]}")
    print(f"Decrypted: {decrypted_texts[i]}\n")

```
![kripto](https://github.com/user-attachments/assets/7c0d031f-c4c7-43ac-a306-3abb593a51a6)



