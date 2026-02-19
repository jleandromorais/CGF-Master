## CGF – Somatório de Volume Faturado

Aplicativo desktop em **Python + Tkinter** para calcular, de forma padronizada, o **volume final CGF** a partir de três planilhas Excel do mês:

- `NF Faturada e complementar.xlsx`
- `NF canceladas e denegadas.xlsx`
- `NF devolução dez.25.xlsx`

O sistema consolida os dados e entrega **um único número de volume** já pronto para relatório/regulador:

- **Somando** o volume faturado para clientes.
- **Descontando** canceladas, denegadas, devoluções e consumo próprio.

---

## Visão geral rápida

- **Entrada**: 3 arquivos Excel (faturadas, canceladas, devoluções).
- **Configuração**: você informa só os **nomes das colunas** conforme o cabeçalho do Excel.
- **Saída**: `Volume Final CGF` + log detalhado do que foi somado e subtraído.

Fórmula de negócio utilizada:

\[
\text{Volume Final CGF} =
\text{Faturado (sem consumo próprio)}
- \text{Canceladas}
- \text{Devoluções}
- \text{Consumo Próprio}
\]

---

## 1. Requisitos

- **Python**: 3.9 ou superior.
- **Bibliotecas Python**:
  - `pandas`
  - `openpyxl` (para ler arquivos `.xlsx`)

Instalação rápida (no PowerShell, dentro da pasta do projeto):

```bash
pip install pandas openpyxl
```

---

## 2. Como rodar

1. Abra o PowerShell na pasta `CGF`.
2. Execute:

```bash
python CGF..py
```

3. A janela do sistema será aberta.

---

## 3. Interface – passo a passo

### 3.1 Card **“Arquivos do mês”**

- **Carregar padrões**: usa os caminhos definidos em `DEFAULT_FILES` (no início de `CGF..py`).
- **Selecionar...**: escolha manualmente arquivos `.xlsx` ou `.csv` do mês.
- **Limpar**: esvazia a lista de arquivos carregados.

Você pode:

- Trabalhar sempre com os **caminhos padrão**, ou
- Selecionar manualmente os arquivos de qualquer pasta/mês.

### 3.2 Abas de configuração por planilha

Na parte inferior esquerda há 3 abas, uma para cada tipo de arquivo:

- **NF Faturada**
- **NF Canceladas**
- **NF Devolução**

Em todas as abas a regra é a mesma: preencher o **nome exato da coluna** conforme o cabeçalho do Excel.

#### Aba **NF Faturada**

- **Coluna de volume faturado**  
  Cabeçalho da coluna com o volume faturado (ex.: `Volume Faturado`).

- **Coluna que indica consumo próprio**  
  Coluna onde aparece a descrição/situação da NF que identifica consumo próprio (ex.: `Descricao`).  

- **Texto exato para consumo próprio**  
  Texto que aparece nessa coluna para marcar consumo próprio (ex.: `CONSUMO PROPRIO`).  
  Todas as linhas com esse texto serão **separadas e subtraídas**.

- **(Opcional) Coluna CFOP**  
  Se preenchida, o CFOP é lido e exibido no log para conferência.

- **Colunas extras (opcional)**  
  Lista de outras colunas que você quer apenas verificar se existem.  
  Elas **não entram na conta**, servem só para checagem no log (ex.: `CLIENTE`, `MUNICIPIO`, etc.).

#### Aba **NF Canceladas**

- **Coluna de volume (canceladas)**  
  Cabeçalho da coluna com o volume das NFs canceladas/denegadas.

- **Colunas extras (opcional)**  
  Apenas para checar se certas colunas existem; não alteram o cálculo.

#### Aba **NF Devolução**

- **Coluna de volume (devoluções)**  
  Cabeçalho da coluna com o volume devolvido.

- **Colunas extras (opcional)**  
  Idem acima, apenas conferência.

---

## 4. O que o sistema calcula exatamente

Depois de configurar as colunas, clique em **CALCULAR** (card da direita).

O fluxo interno é:

1. **NF Faturada e complementar**
   - Converte a coluna de volume para número.
   - Separa as linhas marcadas como **consumo próprio**:
     - Somatório de **faturado (sem consumo próprio)** → entra positivo.
     - Somatório de **consumo próprio** → entra negativo.
2. **NF canceladas e denegadas**
   - Converte a coluna de volume para número.
   - Soma o volume total de canceladas/denegadas → entra negativo.
3. **NF devolução**
   - Converte a coluna de volume para número.
   - Soma o volume total devolvido → entra negativo.
4. **Resumo final**
   - Mostra no log todos os parciais e o **Volume Final CGF** em destaque.

---

## 5. Ajustando para outros meses ou pastas

Você tem duas formas de trocar os arquivos do mês:

- **Pela interface**  
  Usar o botão **Selecionar...** e escolher manualmente os novos arquivos.

- **Pelo código (padrões automáticos)**  
  Alterar a lista `DEFAULT_FILES` no início de `CGF..py`:

```python
DEFAULT_FILES = [
    r"...\NF Faturada e complementar.xlsx",
    r"...\NF canceladas e denegadas.xlsx",
    r"...\NF devolução dez.25.xlsx",
]
```

---

## 6. Troubleshooting (erros comuns)

- **“Coluna de volume não encontrada”**  
  - Verifique se o nome digitado na tela é **idêntico** ao cabeçalho do Excel (acentos, maiúsculas/minúsculas, espaços).

- **Volume final muito diferente do esperado**  
  - Teste com poucas linhas (filtrando no Excel) e faça a conta manual.
  - Confirme:
    - Coluna de volume correta em cada aba.
    - Nome da coluna de consumo próprio.
    - Texto exato de consumo próprio.

- **Erro ao abrir arquivo**  
  - Confira se nenhum dos arquivos está aberto bloqueando gravação/leitura.
  - Verifique se a extensão é suportada (`.xlsx`, `.xls` ou `.csv`).

---

Se você quiser evoluir esse sistema (exportar o log para Excel, salvar presets de configuração por mês, gerar gráficos, etc.), a base já está preparada para isso. É só pedir. 😉
