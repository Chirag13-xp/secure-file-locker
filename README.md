# secure-file-locker
A simple Python tool to encrypt and decrypt files using a password.

from cryptography.fernet import Fernet
import os
from getpass import getpass

def generate_key(password):
    return Fernet(Fernet.generate_key())

def encrypt_file(filename, fernet):
    with open(filename, 'rb') as file:
        original = file.read()
    encrypted = fernet.encrypt(original)
    with open(filename + ".locked", 'wb') as encrypted_file:
        encrypted_file.write(encrypted)
    print(f"✅ File encrypted as {filename}.locked")

def decrypt_file(filename, fernet):
    with open(filename, 'rb') as enc_file:
        encrypted = enc_file.read()
    decrypted = fernet.decrypt(encrypted)
    out_file = filename.replace(".locked", ".unlocked")
    with open(out_file, 'wb') as dec_file:
        dec_file.write(decrypted)
    print(f"✅ File decrypted as {out_file}")

def main():
    print("🔐 Secure File Locker")
    choice = input("Do you want to (E)ncrypt or (D)ecrypt a file? ").lower()

    if choice not in ['e', 'd']:
        print("❌ Invalid choice.")
        return

    filename = input("Enter file name: ")
    if not os.path.exists(filename):
        print("❌ File does not exist.")
        return

    password = getpass("Enter password: ")
    fernet = generate_key(password)  # Simplified key gen

    try:
        if choice == 'e':
            encrypt_file(filename, fernet)
        else:
            decrypt_file(filename, fernet)
    except Exception as e:
        print("❌ Error:", e)

if __name__ == "__main__":
    main()
