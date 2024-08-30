## Detecção de Placas Arduino

Este projeto utiliza o `arduino-cli` para detectar placas Arduino conectadas ao computador. O script verifica as portas seriais disponíveis e identifica as placas Arduino Uno e Mega 2560.

### Como Funciona

1. **Execução do Comando de Detecção:**
   O script usa o comando `arduino-cli board list` para listar as placas conectadas e suas portas seriais.

2. **Identificação das Placas:**
   O script analisa a saída para identificar se as placas conectadas são Arduino Uno ou Mega 2560, com base na identificação do chip (`ATmega328P` para Uno e `ATmega2560` para Mega 2560).

3. **Retorno dos Resultados:**
   A função `detect_arduino_port()` retorna um dicionário onde a chave é a porta COM e o valor é o Fully Qualified Board Name (FQBN) correspondente, por exemplo, `arduino:avr:uno` para Arduino Uno e `arduino:avr:mega` para Mega 2560.

### Código de Exemplo

```python
import subprocess

def detect_arduino_port():
    """Detecta a porta onde a Arduino Uno ou Mega 2560 está conectada."""
    try:
        result = subprocess.run(['arduino-cli', 'board', 'list'], capture_output=True, text=True, creationflags=subprocess.CREATE_NO_WINDOW)
        if result.returncode == 0:
            ports = {}
            for line in result.stdout.splitlines():
                if 'ATmega328P' in line:  # Usado pela Uno
                    port = line.split()[0]  # Porta COM para Uno
                    ports[port] = 'arduino:avr:uno'
                elif 'ATmega2560' in line:  # Usado pela Mega 2560
                    port = line.split()[0]  # Porta COM para Mega 2560
                    ports[port] = 'arduino:avr:mega'
            return ports if ports else None
        return None
    except FileNotFoundError:
        return None
    except Exception as e:
        print(f"Erro ao detectar porta: {e}")
        return None
```

### Uso
Chame a função detect_arduino_port() para obter uma lista de portas e placas detectadas. Se a função retornar um dicionário vazio, nenhuma placa compatível foi encontrada.

```python
ports = detect_arduino_port()
if ports:
    print("Placas detectadas:", ports)
else:
    print("Nenhuma placa Arduino detectada.")
```

Esse trecho de Markdown fornece uma visão geral clara sobre a detecção de placas Arduino, incluindo o funcionamento básico, o código de exemplo e como utilizar a função no seu projeto.