# Computação Quântica Aplicada a Problemas de Inteligência Artificial: notebooks complementares

Repositório complementar ao trabalho de conclusão de curso Computação Quântica Aplicada a Problemas de Inteligência Artificial. Ele reúne três notebooks didáticos para estudantes de graduação. Os notebooks implementam, executam e comparam algoritmos clássicos e quânticos em instâncias pequenas. Os componentes quânticos são simulados com [PennyLane](https://docs.pennylane.ai/en/stable/) (`default.qubit`) em um computador comum, sem GPU, hardware quântico, tokens ou serviços pagos.

## Conteúdo

| Notebook                                       | Tópicos                                                                                                                                                                                                                                                                                                                                                                                                             |
| ---------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `notebooks/01_busca_classica_quantica.ipynb` | Busca exaustiva e aleatorizada; Grover, com interpretação geométrica, domínio não potência de dois e BBHT; mínimo de Dürr e Høyer, com oráculo reversível e orçamento; BFS, DFS, A* e passeio aleatório em um toro implícito; DTQW na reta, no hipercubo e no toro; Minimax, redução AND-OR e fórmulas NAND; CTQW na reta e no toro; avaliação NAND por espalhamento (Farhi, Goldstone e Gutmann) |
| `notebooks/02_aprendizado_iris.ipynb`        | Iris: SVM linear, SVM RBF, MLP, QSVM com kernel de fidelidade, VQC com recarregamento de dados, platôs estéreis                                                                                                                                                                                                                                                                                                   |

## Instalação

São necessários Python 3.13. Os pacotes necessários estão em `requirements.txt`.

**Windows (PowerShell)**

```powershell
py -3.13 -m venv .venv
.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

**Linux / macOS**

```bash
python3.13 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

## Execução

**Interativa**

```bash
jupyter lab
```

Abra um notebook em `notebooks/` e execute *Kernel → Restart Kernel and Run All Cells*. Cada notebook é independente e pode ser executado sozinho, em qualquer ordem.

**Automatizada (todos os notebooks, modo rápido)**, a partir da raiz do repositório:

```bash
jupyter nbconvert --to notebook --execute --inplace --ExecutePreprocessor.timeout=3600 notebooks/01_busca_classica_quantica.ipynb notebooks/02_aprendizado_iris.ipynb notebooks/03_aprendizado_wine_quality.ipynb
```

**Modo completo.** Cada notebook tem `MODO = "rapido"` como padrão. O modo completo usa mais repetições, instâncias maiores, várias sementes de divisão e o Wine Quality inteiro. Para ativá-lo sem editar os notebooks, defina a variável de ambiente antes de iniciar o Jupyter ou o `nbconvert`:

```powershell
$env:TCC_MODO = "completo"   # Windows (PowerShell)
```

```bash
export TCC_MODO=completo     # Linux / macOS
```

O modo completo demora mais que o rápido. Os tempos medidos do modo rápido estão em `VALIDATION.md`.

## Dados

- **Iris:** carregado de `sklearn.datasets.load_iris`, sem download.

## Saídas

Os notebooks criam automaticamente `outputs/<notebook>/figuras/` (PNG) e `outputs/<notebook>/tabelas/` (CSV). Esses diretórios são regenerados a cada execução e estão no `.gitignore`. Os notebooks versionados já contêm as saídas da última execução do modo rápido.

## Organização

```
.
├── README.md
├── requirements.txt
├── .gitignore
├── data/
└── notebooks/
    ├── 01_busca_classica_quantica.ipynb
    └── 02_aprendizado_iris.ipynb
```

## Limites

- As simulações usam no máximo 10 qubits. Os resultados ilustram mecanismos e não demonstram vantagem quântica nem aceleração assintótica.
- Custos em consultas e em tempo hamiltoniano seguem os modelos teóricos citados. Os tempos de parede medem a simulação clássica.
- O oráculo de Dürr e Høyer nas instâncias maiores é idealizado e montado lendo a tabela de custos. A CTQW usa a exponencial matricial densa (`qml.QubitUnitary`), uma simulação didática, sem síntese eficiente em portas.
- Os experimentos de aprendizado usam amostras pequenas. As diferenças entre modelos devem ser lidas junto com o erro padrão e, no modo completo, com a dispersão entre sementes.
- O treinamento do VQC usa a diferenciação automática do simulador (`backprop`), indisponível em hardware. A regra de deslocamento de parâmetros é demonstrada e conferida à parte.
