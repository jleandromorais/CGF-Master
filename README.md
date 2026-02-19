## CGF - Somatório de Volume Faturado

Aplicativo desktop em **Python + Tkinter** para calcular o **volume final CGF** a partir de três planilhas Excel:

- `NF Faturada e complementar.xlsx`
- `NF canceladas e denegadas.xlsx`
- `NF devolução dez.25.xlsx`

O sistema:

- Soma o **volume faturado** da NF faturada/complementar.
- Subtrai o volume de **canceladas/denegadas**.
- Subtrai o volume de **devoluções**.
- Subtrai o volume de **consumo próprio** dentro da NF faturada (linha identificada por texto na coluna escolhida).

---

### 1. Requisitos

- Python 3.9 ou superior instalado (no Windows já está ok na sua máquina).
- Bibliotecas Python:
  - `pandas`
  - `openpyxl` (para ler `.xlsx`)

Instalação rápida (no PowerShell, dentro da pasta do projeto):

```bash
pip install pandas openpyxl
```

---

### 2. Como rodar o programa

1. Abra o PowerShell na pasta `CGF`.
2. Execute:

```bash
python CGF..py
```

3. A janela do sistema CGF abrirá.

---

### 3. Uso da interface

#### 3.1. Card “Arquivos do mês”

- **Carregar padrões**: carrega automaticamente os três caminhos configurados no código (`DEFAULT_FILES`).
- **Selecionar...**: permite escolher manualmente arquivos `.xlsx` ou `.csv`.
- **Limpar**: zera a lista de arquivos.

Você pode usar tanto os caminhos padrão quanto escolher manualmente outros arquivos do mês.

#### 3.2. Abas de configuração

Na parte de baixo esquerda existem 3 abas:

- **NF Faturada**
- **NF Canceladas**
- **NF Devolução**

Em cada aba você informa **o nome exato das colunas do Excel** (cabeçalho) para aquele arquivo.

##### NF Faturada

- **Coluna de volume faturado**: cabeçalho da coluna que guarda o volume da NF (ex.: `Volume Faturado`).
- **Coluna que indica consumo próprio**: coluna onde aparece o texto de consumo próprio (ex.: `Descricao`).
- **Texto exato para consumo próprio**: texto que identifica as linhas de consumo próprio (ex.: `CONSUMO PROPRIO`).
- **(Opcional) Coluna CFOP**: se quiser usar/visualizar o CFOP no log.
- **Colunas extras (opcional)**: lista de outras colunas que você quer apenas verificar se existem (para conferência). Não entram no cálculo, só aparecem no log.

##### NF Canceladas

- **Coluna de volume (canceladas)**: cabeçalho da coluna com o volume de notas canceladas/denegadas.
- **Colunas extras (opcional)**: campos só para checar se colunas existem; não mudam o cálculo.

##### NF Devolução

- **Coluna de volume (devoluções)**: cabeçalho da coluna com o volume devolvido.
- **Colunas extras (opcional)**: idem acima, apenas conferência.

---

### 4. Cálculo realizado

Depois de configurar as colunas para cada planilha, clique no botão **CALCULAR** (card da direita).

O programa faz:

1. **NF Faturada e complementar**
   - Soma o volume da coluna de **volume faturado**, excluindo as linhas marcadas como **consumo próprio**.
2. **NF canceladas e denegadas**
   - Soma o volume da coluna informada e **subtrai** do total.
3. **NF devolução**
   - Soma o volume da coluna informada e **subtrai** do total.
4. **Consumo próprio**
   - O volume das linhas de consumo próprio é **subtraído** do faturado.

Fórmula final:

\[
\text{Volume Final CGF} =
\text{Faturado (sem consumo próprio)}
- \text{Canceladas}
- \text{Devoluções}
- \text{Consumo Próprio}
\]

O resultado aparece em destaque como **“Volume Final CGF”** e o log à direita mostra os detalhes de cada arquivo processado.

---

### 5. Ajustando para outros meses / pastas

Se você trocar o mês ou a pasta dos arquivos, existem duas opções:

- Usar **Selecionar...** e escolher manualmente os novos arquivos.
- Alterar os caminhos em `DEFAULT_FILES` no começo do arquivo `CGF..py`.

```python
DEFAULT_FILES = [
    r"...\NF Faturada e complementar.xlsx",
    r"...\NF canceladas e denegadas.xlsx",
    r"...\NF devolução dez.25.xlsx",
]
```

---

### 6. Problemas comuns

- **Erro: coluna não encontrada**  
  Verifique se o nome digitado na tela é exatamente igual ao cabeçalho do Excel (acentos, espaços, maiúsculas/minúsculas).

- **Volume final estranho**  
  Teste com poucas linhas (filtrando no Excel) e compare a conta manual com o que o sistema retorna. Ajuste nomes de colunas e texto de consumo próprio se necessário.

Se quiser evoluir o sistema (exportar relatório, salvar configurações por mês, etc.), é só pedir. :)

