# Endpoint `/api/core_list`

## Descrição

O endpoint `/api/core_list` é utilizado para listar todos os núcleos (cores) instalados no ambiente Arduino. Ele faz isso executando o comando `arduino-cli core list` e retorna a lista dos núcleos disponíveis.

## Código

```python
@app.route('/api/core_list', methods=['GET'])
def list_cores():
    output = run_command(['arduino-cli', 'core', 'list'])
    return jsonify({"cores": output})
```

# Endpoint `/api/install_core`

## Descrição

O endpoint `/api/install_core` é utilizado para instalar um núcleo (core) específico no ambiente Arduino. Ele faz isso executando o comando `arduino-cli core install` com o identificador do núcleo fornecido no corpo da solicitação. O comando `arduino-cli` é usado para gerenciar os núcleos, e o endpoint retorna uma mensagem com o resultado da instalação.

## Código

```python
@app.route('/api/install_core', methods=['POST'])
def install_core():
    data = request.json
    core_id = data.get('core_id')
    output = run_command(['arduino-cli', 'core', 'install', core_id])
    return jsonify({"message": output})
```

# Endpoint `/api/uninstall_core`

## Descrição

O endpoint `/api/uninstall_core` é utilizado para desinstalar um núcleo (core) específico do ambiente Arduino. Ele faz isso executando o comando `arduino-cli core uninstall` com o identificador do núcleo fornecido no corpo da solicitação. O comando `arduino-cli` gerencia a remoção do núcleo, e o endpoint retorna uma mensagem com o resultado da desinstalação.

## Código

```python
@app.route('/api/uninstall_core', methods=['POST'])
def uninstall_core():
    data = request.json
    core_id = data.get('core_id')
    output = run_command(['arduino-cli', 'core', 'uninstall', core_id])
    return jsonify({"message": output})
```

# Endpoint `/api/update_core`

## Descrição

O endpoint `/api/update_core` é utilizado para atualizar um núcleo (core) específico do ambiente Arduino. Ele faz isso executando o comando `arduino-cli core upgrade` com o identificador do núcleo fornecido no corpo da solicitação. O comando `arduino-cli` gerencia a atualização do núcleo, e o endpoint retorna uma mensagem com o resultado da atualização.

## Código

```python
@app.route('/api/update_core', methods=['POST'])
def update_core():
    data = request.json
    core_id = data.get('core_id')
    output = run_command(['arduino-cli', 'core', 'upgrade', core_id])
    return jsonify({"message": output})


