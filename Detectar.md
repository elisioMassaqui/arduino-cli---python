# Detecção de Placas Arduino

Este projeto utiliza o `arduino-cli` para detectar placas Arduino conectadas ao computador. O script verifica as portas seriais disponíveis e identifica as placas Arduino Uno e Mega 2560.

## Como Funciona

1. **Execução do Comando de Detecção:**
   O script usa o comando `arduino-cli board list` para listar as placas conectadas e suas portas seriais.

2. **Identificação das Placas:**
   O script analisa a saída para identificar se as placas conectadas são Arduino Uno ou Mega 2560, com base na identificação do chip (`ATmega328P` para Uno e `ATmega2560` para Mega 2560).

3. **Retorno dos Resultados:**
   A função `detect_arduino_port()` retorna o nome da porta COM detectada. Se uma placa Arduino Uno ou Mega 2560 for encontrada, o nome da porta é exibido no final.

## Código de Exemplo

```python
import subprocess

def detect_arduino_port():
    """Detecta a porta onde a Arduino Uno ou Mega 2560 está conectada e exibe o nome da porta no final."""
    try:
        print("Executando comando para listar portas...")
        result = subprocess.run(['arduino-cli', 'board', 'list'], capture_output=True, text=True, creationflags=subprocess.CREATE_NO_WINDOW)
        
        if result.returncode == 0:
            print("Comando executado com sucesso. Analisando portas...")
            detected_port = None
            
            for line in result.stdout.splitlines():
                print(f"Linha capturada: {line}")
                if 'ATmega328P' in line or 'ATmega2560' in line:  # Verifica se a linha contém informações da Uno ou Mega
                    detected_port = line.split()[0]  # Armazena a porta detectada
                    break  # Para após encontrar a primeira placa
            
            if detected_port:
                print(f"Porta detectada: {detected_port}")
            else:
                print("Nenhuma placa Arduino Uno ou Mega 2560 foi detectada.")
        else:
            print("Falha ao executar o comando.")
    except FileNotFoundError:
        print("arduino-cli não encontrado. Verifique se está instalado e no PATH.")
    except Exception as e:
        print(f"Erro ao detectar porta: {e}")
    finally:
        input("Pressione Enter para sair...")  # Mantém a janela aberta para ver as mensagens de erro

# Chama a função para testar
detect_arduino_port()
```

Uso
Chame a função detect_arduino_port() para obter o nome da porta detectada. Se a função encontrar uma placa Arduino Uno ou Mega 2560, ela exibirá o nome da porta no final.

```python
detect_arduino_port()
```
Se a função não detectar nenhuma placa compatível, a mensagem "Nenhuma placa Arduino Uno ou Mega 2560 foi detectada." será exibida.

### Explicação do Markdown:

- **Título e Descrição:** Descreve o propósito do projeto e como ele funciona.
- **Como Funciona:** Explica o processo de detecção e identificação das placas.
- **Código de Exemplo:** Inclui o código atualizado que detecta e exibe o nome da porta.
- **Uso:** Mostra como usar a função e o que esperar como resultado.

Adapte o conteúdo conforme necessário para se ajustar ao estilo e às necessidades específicas do seu projeto.
