# Detecção de Placas Arduino

Este projeto utiliza o `arduino-cli` para detectar placas Arduino conectadas ao computador. O script verifica as portas seriais disponíveis e identifica as placas Arduino.

## Como Funciona

1. **Execução do Comando de Detecção:**
   O script usa o comando `arduino-cli board list` para listar as placas conectadas e suas portas seriais.

## Código de Exemplo

```python
import subprocess

def detect_any_port():
    """Detecta qualquer porta onde um dispositivo está conectado e exibe o nome da porta no final."""
    try:
        print("Executando comando para listar portas...")
        result = subprocess.run(['arduino-cli', 'board', 'list'], capture_output=True, text=True, creationflags=subprocess.CREATE_NO_WINDOW)
        
        if result.returncode == 0:
            print("Comando executado com sucesso. Analisando portas...")
            detected_port = None
            
            for line in result.stdout.splitlines():
                print(f"Linha capturada: {line}")
                if 'COM' in line or '/dev' in line:  # Verifica se a linha contém uma porta COM ou /dev
                    detected_port = line.split()[0]  # Armazena a porta detectada
                    break  # Para após encontrar a primeira porta
            
            if detected_port:
                print(f"Porta detectada: {detected_port}")
            else:
                print("Nenhuma porta foi detectada.")
        else:
            print("Falha ao executar o comando.")
    except FileNotFoundError:
        print("arduino-cli não encontrado. Verifique se está instalado e no PATH.")
    except Exception as e:
        print(f"Erro ao detectar porta: {e}")
    finally:
        input("Pressione Enter para sair...")  # Mantém a janela aberta para ver as mensagens de erro

# Chama a função para testar
detect_any_port()

```

Uso
Chame a função detect_arduino_port() para obter o nome da porta detectada. Se a função encontrar uma placa Arduino

```python
detect_arduino_port()
```
