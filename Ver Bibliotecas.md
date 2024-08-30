## Visualização de Bibliotecas Arduino

Este projeto usa o `arduino-cli` para buscar e listar bibliotecas disponíveis para uso com placas Arduino. A função `get_libraries_from_arduino_cli()` coleta informações sobre as bibliotecas e as exibe através de uma API.

### Como Funciona

1. **Execução do Comando de Busca:**
   O script utiliza o comando `arduino-cli lib search` para buscar informações sobre as bibliotecas disponíveis. O resultado é processado para extrair detalhes sobre cada biblioteca.

2. **Processamento da Saída:**
   A saída do comando é analisada usando expressões regulares para extrair informações como nome, autor, categoria e a última versão disponível de cada biblioteca.

3. **Retorno dos Resultados:**
   A função `api_libraries()` expõe os resultados da busca através de uma API que retorna uma lista de bibliotecas em formato JSON.

### Código de Exemplo

```python
import subprocess
import re
from flask import jsonify

def get_libraries_from_arduino_cli():
    try:
        result = subprocess.run(
            ["arduino-cli", "lib", "search"], 
            stdout=subprocess.PIPE, 
            stderr=subprocess.PIPE, 
            text=True,
            encoding='utf-8',
            creationflags=subprocess.CREATE_NO_WINDOW
        )

        output = result.stdout
        libraries = []
        pattern = re.compile(
            r'Name:\s*(.*?)\n\s*Author:\s*(.*?)\n.*?Category:\s*(.*?)\n.*?Versions:\s*\[(.*?)\]',
            re.DOTALL
        )

        matches = pattern.finditer(output)
        for match in matches:
            name = match.group(1).strip()
            author = match.group(2).strip()
            category = match.group(3).strip()
            versions = match.group(4).strip().split(',')
            last_version = versions[-1].strip() if versions else "N/A"

            libraries.append({
                'Name': name,
                'Author': author,
                'Category': category,
                'LastVersion': last_version
            })

        return libraries

    except Exception as e:
        print(f"Erro ao executar o comando arduino-cli: {e}")
        return []

@app.route('/api/libraries')
def api_libraries():
    libraries = get_libraries_from_arduino_cli()
    return jsonify(libraries)
```

Uso
Acesse a rota /api/libraries para obter uma lista das bibliotecas disponíveis no formato JSON. A API retorna informações sobre cada biblioteca, incluindo o nome, autor, categoria e a última versão disponível.

Exemplo de Solicitação:

```bash
curl http://127.0.0.1:5000/api/libraries
```
Exemplo de Resposta:

```json
[
    {
        "Name": "Library Name",
        "Author": "Library Author",
        "Category": "Library Category",
        "LastVersion": "1.0.0"
    },
    ...
]
```

Erros Comuns
Erro ao Executar o Comando: Verifique se o arduino-cli está instalado e configurado corretamente.
Problemas de Análise: Certifique-se de que a saída do comando está no formato esperado e que as expressões regulares estão corretas.

Essa descrição fornece uma visão geral do processo de visualização de bibliotecas, incluindo o funcionamento básico, o código de exemplo e como utilizar a API para obter informações sobre as bibliotecas disponíveis.