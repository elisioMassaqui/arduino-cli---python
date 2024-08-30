## Compilação de Código para Placas Arduino

Este projeto usa o `arduino-cli` para compilar código Arduino para placas conectadas, como Arduino Uno e Mega 2560. A compilação converte o código-fonte `.ino` em um binário que pode ser carregado na placa.

### Como Funciona

1. **Execução do Comando de Compilação:**
   O script usa o comando `arduino-cli compile` para compilar o código-fonte do projeto. O Fully Qualified Board Name (FQBN) da placa detectada é utilizado para especificar a placa alvo durante a compilação.

2. **Seleção da Placa:**
   Antes de compilar, o script identifica a placa conectada e sua porta serial. Baseado na detecção, o FQBN apropriado é selecionado para a compilação.

3. **Retorno dos Resultados:**
   A função `compile_code()` executa a compilação e retorna o resultado, incluindo a saída do comando e quaisquer erros encontrados durante o processo.

### Código de Exemplo

```python
import subprocess
from flask import request, jsonify

ARDUINO_CLI_PATH = 'arduino-cli'

@app.route('/api/compile_code', methods=['POST'])
def compile_code():
    project_name = request.json.get('project_name')
    project_path = get_project_path(project_name)
    file_path = os.path.join(project_path, f"{project_name}.ino")
    if os.path.exists(file_path):
        ports = detect_arduino_port()
        if ports:
            port, fqbn = next(iter(ports.items()))
            command = [ARDUINO_CLI_PATH, 'compile', '--fqbn', fqbn, file_path]
            result = subprocess.run(command, capture_output=True, text=True, creationflags=subprocess.CREATE_NO_WINDOW)
            if result.returncode == 0:
                return jsonify({"message": "Compilação concluída com sucesso!", "output": result.stdout})
            else:
                return jsonify({"message": "Erro na compilação.", "error": result.stderr}), 500
    return jsonify({"message": "Projeto não encontrado ou porta não detectada!"}), 404
```

Uso
Envie uma solicitação POST para a rota /api/compile_code com um JSON contendo o nome do projeto para compilar. A resposta incluirá uma mensagem de sucesso ou erro, além da saída da compilação.

Exemplo de Solicitação:

```bash
curl -X POST http://127.0.0.1:5000/api/compile_code -H "Content-Type: application/json" -d '{"project_name": "meu_projeto"}'
```
Exemplo de Resposta:

```json
{
    "message": "Compilação concluída com sucesso!",
    "output": "Compiling sketch...\n..."
}
```

Erros Comuns
Erro na Detecção da Placa: Verifique se a placa está conectada corretamente e se o arduino-cli está configurado.
Erro de Compilação: Revise o código-fonte e as mensagens de erro retornadas para identificar problemas específicos no código.

Este trecho fornece uma visão geral do processo de compilação, incluindo o funcionamento básico, o código de exemplo e como utilizar a função de compilação no seu projeto.