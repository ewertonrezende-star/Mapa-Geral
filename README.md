# Mapa Geral – Cooperativa Agrícola de Sintra

Aplicação web para **mapa de stock por loja**, com base de fornecedores fixa e listagem apenas de artigos com **vendas nos últimos 30 dias**.

**URL:** [https://ewertonrezende-star.github.io/Mapa-Geral/](https://ewertonrezende-star.github.io/Mapa-Geral/)

---

## O que faz

| Função | Descrição |
|--------|-----------|
| **Central / Compras** | Cria o mapa, publica ficheiros de vendas (30 dias + período homólogo) por loja e partilha o link |
| **Loja** | Escolhe a loja, vê fornecedores com vendas, preenche stock real / alterações, guarda online e exporta Excel |

- Catálogo de artigos e fornecedores **embutido** (não é necessário carregar stock geral).
- Só aparecem artigos com **vendas 30 dias > 0** na loja em causa.
- O fornecedor de cada artigo vem da base fixa.
- Dados guardados no **Supabase** (tabela `mapas_geral`).
- Pode ser instalada como **app** (PWA) no telemóvel ou PC.

---

## Como usar

### 1. Central cria o mapa

1. Abre a app (código do mapa **vazio** = criar novo).
2. Entra em **Central / Compras** com a senha da central.
3. Copia o **código** ou o **link** e envia às lojas.
4. Por cada loja, carrega:
   - ficheiro **Vendas 30 dias** (`.xlsx` / `.csv`)
   - ficheiro **Vendas período homólogo** (opcional)

Formato típico Primavera: colunas Artigo, Descrição, STK. Actual, vendas… — a app usa a coluna de **Vendas Ano Corrente** como vendas 30 dias e o stock da coluna de stock quando existir.

### 2. Loja preenche o mapa

1. Abre o **link** enviado pela Central (ou cola o código).
2. Escolhe a **loja** e a senha da loja.
3. Seleciona o **fornecedor** na lista (só os que têm artigos com vendas 30 dias).
4. Preenche **Stock Real** (e **Alteração** se precisar).
5. **Guardar online** — um fornecedor de cada vez.
6. Quando todos os fornecedores estiverem guardados, pode **Enviar por email** (gera Excel com uma folha por fornecedor).

### 3. Lojas disponíveis

| Código | Loja |
|--------|------|
| AL | Albarraque |
| AM | Arneiro Marinheiro |
| CO | Colares |
| MM | Mem Martins |
| LC | Sintra |
| SA | Sabugo |

---

## Senhas neste computador

Após entrar corretamente uma vez, a app **lembra** a autenticação neste browser/PC (Central e cada loja).

- Botão **“Esquecer senhas neste computador”** na página inicial para limpar.
- Os dados ficam só no `localStorage` deste dispositivo (`mapa_geral_auth_v1`).

---

## Instalar como app (PWA)

1. Abre o site no telemóvel ou PC.
2. Usa **Instalar app no telemóvel / PC** (ou o menu do browser → Instalar).
3. No iPhone: Partilhar → **Adicionar ao Ecrã Principal**.

Ficheiros necessários no repositório:

```
Mapa-Geral/
├── index.html
├── manifest.webmanifest
├── sw.js
├── logo-coopsintra.png
└── icons/
    ├── icon-192.png
    └── icon-512.png
```

---

## Estrutura do projeto

| Ficheiro | Função |
|----------|--------|
| `index.html` | Aplicação completa (UI + lógica + catálogo fixo) |
| `manifest.webmanifest` | Nome/ícone da app instalável (**Mapa Geral**) |
| `sw.js` | Service worker (cache offline básico) |
| `icons/` | Ícones PWA (fundo vermelho para distinguir do Mapa Stock) |
| `logo-coopsintra.png` | Logo no ecrã de entrada |

---

## Supabase

- Projeto: o mesmo da cooperativa (`coopsintra-stock`).
- Tabela: **`mapas_geral`**
- A Data API deve ter a tabela **exposta** e políticas RLS que permitam `SELECT` / `INSERT` / `UPDATE` para `anon` (ou equivalente para uso interno).

Cada mapa é um registo com `id` (código) e JSON `lojas` (vendas + respostas).

---

## Diferença face ao Mapa Stock antigo

| | Mapa Stock (`coopsintra`) | Mapa Geral |
|--|--------------------------|------------|
| Base de fornecedores | Upload de stock geral | **Fixa** no código |
| Filtro de artigos | Conforme stock/vendas carregados | **Só vendas 30 dias** por loja |
| Homólogo | Não no mesmo formato | Coluna **Vendas Homólogo** |
| Tabela Supabase | `mapas_stock` | `mapas_geral` |
| Nome da app | Mapa Stock | **Mapa Geral** |
| Ícone | Fundo azul | Fundo **vermelho** |

---

## Desenvolvimento local

Abrir `index.html` num servidor local (necessário para PWA / alguns browsers):

```bash
npx serve .
# ou
python3 -m http.server 8080
```

Depois aceder a `http://localhost:8080`.

---

## Notas

- Cada loja só vê artigos com vendas **nessa** loja; o mesmo fornecedor pode ter listas diferentes em Sintra e Colares.
- A previsão de encomenda usa lead time (predefinido 30 dias) e cobertura ≈ 2 × lead time.
- Exportação Excel e envio por email usam a biblioteca SheetJS (CDN).

---

## Licença / uso

Uso interno da **Cooperativa Agrícola de Sintra**.
