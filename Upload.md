## Envio de Código para Placas Arduino

Este projeto usa o `arduino-cli` para enviar código compilado para placas Arduino conectadas, como Arduino Uno e Mega 2560. O envio faz o upload do binário compilado para a placa para que o código possa ser executado.

### Como Funciona

1. **Execução do Comando de Upload:**
   O script usa o comando `arduino-cli upload` para enviar o código compilado para a placa. O Fully Qualified Board Name (FQBN) da placa e a porta serial onde a placa está conectada são utilizados para o upload.

2. **Seleção da Placa e Porta:**
   Antes de fazer o upload, o script identifica a placa conectada e sua porta serial. A placa é selecionada com base na detecção anterior, e a porta é usada para o envio do código.

3. **Retorno dos Resultados:**
   A função `upload_code()` executa o upload e retorna o resultado, incluindo a saída do comando e quaisquer erros encontrados durante o processo.

### Código de Exemplo

```python
import subprocess
from flask import request, jsonify

ARDUINO_CLI_PATH = 'arduino-cli'

@app.route('/api/upload_code', methods=['POST'])
def upload_code():
    project_name = request.json.get('project_name')
    project_path = get_project_path(project_name)
    file_path = os.path.join(project_path, f"{project_name}.ino")
    if os.path.exists(file_path):
        ports = detect_arduino_port()
        if ports:
            port, fqbn = next(iter(ports.items()))
            command = [ARDUINO_CLI_PATH, 'upload', '--fqbn', fqbn, '-p', port, file_path]
            result = subprocess.run(command, capture_output=True, text=True, creationflags=subprocess.CREATE_NO_WINDOW)
            if result.returncode == 0:
                return jsonify({"message": "Upload concluído com sucesso!", "output": result.stdout})
            else:
                return jsonify({"message": "Erro no upload.", "error": result.stderr}), 500
        return jsonify({"message": "Porta Arduino não detectada!"}), 404
    return jsonify({"message": "Projeto não encontrado!"}), 404
```

Uso
Envie uma solicitação POST para a rota /api/upload_code com um JSON contendo o nome do projeto para enviar para a placa. A resposta incluirá uma mensagem de sucesso ou erro, além da saída do comando de upload.

Exemplo de Solicitação:

```bash
curl -X POST http://127.0.0.1:5000/api/upload_code -H "Content-Type: application/json" -d '{"project_name": "meu_projeto"}'
```

Exemplo de Resposta:

```json
{
    "message": "Upload concluído com sucesso!",
    "output": "Uploading...\n..."
}
```

Erros Comuns
Erro na Detecção da Porta: Certifique-se de que a placa está conectada corretamente e que o arduino-cli está configurado corretamente.
Erro no Upload: Verifique a mensagem de erro retornada para identificar possíveis problemas com o código ou a conexão com a placa.

Essa descrição fornece uma visão geral do processo de envio de código, incluindo o funcionamento básico, o código de exemplo e como utilizar a função de upload no seu projeto.