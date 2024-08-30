# Atualizar Índice de Bibliotecas

Esta rota atualiza o índice de bibliotecas para garantir que as informações sobre bibliotecas disponíveis estejam atualizadas.

## Rota

- **URL:** `/api/available_libraries`
- **Método:** `GET`

## Descrição

Atualiza o índice de bibliotecas usando o comando `arduino-cli lib update-index`. Isso é necessário para garantir que a lista de bibliotecas disponíveis esteja atualizada e reflita as bibliotecas recentemente adicionadas ou modificadas.

## Código

```python
@app.route('/api/available_libraries', methods=['GET'])
def list_available_libraries():
    run_command(['arduino-cli', 'lib', 'update-index'])
    return jsonify({"message": "Atualize o índice para obter bibliotecas disponíveis"}), 200
```

# Listar Bibliotecas Instaladas

Esta rota lista todas as bibliotecas que estão atualmente instaladas no ambiente Arduino.

## Rota

- **URL:** `/api/installed_libraries`
- **Método:** `GET`

## Descrição

Obtém uma lista das bibliotecas instaladas usando o comando `arduino-cli lib list`. Esta rota retorna o nome das bibliotecas que estão instaladas no ambiente Arduino.

## Código

```python
@app.route('/api/installed_libraries', methods=['GET'])
def list_installed_libraries():
    output = run_command(['arduino-cli', 'lib', 'list'])
    libraries = [line.split()[0] for line in output.splitlines() if line]
    return jsonify({"libraries": libraries})
```

# Instalar Biblioteca

Esta rota instala uma biblioteca no ambiente Arduino.

## Rota

- **URL:** `/api/install_library`
- **Método:** `POST`

## Descrição

Instala uma biblioteca especificada pelo nome usando o comando `arduino-cli lib install`. O nome da biblioteca deve ser fornecido no corpo da solicitação como um JSON.

## Código

```python
@app.route('/api/install_library', methods=['POST'])
def install_library():
    data = request.json
    library_name = data.get('library_name')
    output = run_command(['arduino-cli', 'lib', 'install', library_name])
    return jsonify({"message": output})
```

# Desinstalar Biblioteca

Esta rota desinstala uma biblioteca do ambiente Arduino.

## Rota

- **URL:** `/api/uninstall_library`
- **Método:** `POST`

## Descrição

Desinstala uma biblioteca especificada pelo nome usando o comando `arduino-cli lib uninstall`. O nome da biblioteca deve ser fornecido no corpo da solicitação como um JSON.

## Código

```python
@app.route('/api/uninstall_library', methods=['POST'])
def uninstall_library():
    data = request.json
    library_name = data.get('library_name')
    output = run_command(['arduino-cli', 'lib', 'uninstall', library_name])
    return jsonify({"message": output})
```