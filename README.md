# ARDUINO-CLI + PYTHON

Este documento aborda a integração do Arduino CLI com Python, destacando os principais comandos do Arduino CLI que foram utilizados para gerenciar núcleos, placas e bibliotecas. Abaixo, estão os comandos que foram integrados com a API Python.

### Comandos de Inicialização para Arduino CLI
Para configurar o ambiente e começar a utilizar o Arduino CLI, os seguintes comandos de inicialização são essenciais:

### Inicializar(Opcional):
```bash
arduino-cli config init
```

### Atualizar o Índice de Núcleos e Bibliotecas
Certifique-se de que o Arduino CLI tenha as informações mais recentes sobre núcleos e bibliotecas:

```bash
arduino-cli core update-index
arduino-cli lib update-index
```

### Instalar o Núcleo Necessário
Instale o núcleo correspondente às placas que você deseja usar (por exemplo, AVR para Arduino Uno e Mega 2560):
```bash
arduino-cli core install arduino:avr
```

### Verificar Núcleos Instalados
Verifique quais núcleos já estão instalados no seu sistema:
```bash
arduino-cli core list
```
Esses comandos garantirão que seu ambiente esteja preparado para o desenvolvimento de projetos com o Arduino CLI.







