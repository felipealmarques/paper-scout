# 📚 Academic Article Search Engine (CLACSO, ERIC, SciELO)

Este projeto tem como objetivo construir um **motor de busca acadêmico automatizado**, focado em repositórios com filtros limitados como **CLACSO, ERIC e SciELO**.

A proposta é simples, mas poderosa:

> Automatizar a coleta, processamento e disponibilização de artigos relevantes — sem necessidade de manutenção contínua.

---

# 🎯 Objetivos

- Automatizar o scraping de múltiplos repositórios acadêmicos  
- Melhorar a qualidade da busca (além de filtros nativos ruins)  
- Permitir busca inteligente (keywords + expansão semântica futura)  
- Criar uma interface simples e acessível  
- Garantir que tudo rode **sem backend ativo**  

---

# 🧠 Arquitetura do Projeto

O sistema segue uma arquitetura **offline-first + frontend estático**:

GitHub Actions (pipeline)
↓
Processamento + NLP
↓
Exportação (JSON)
↓
GitHub Pages (frontend)


---

## 🔄 Pipeline de Dados (Offline)

Executado automaticamente via GitHub Actions.

### Etapas:

1. **Scraping**
   - CLACSO
   - ERIC
   - SciELO

2. **Processamento**
   - Normalização de texto:
     - lowercase
     - remoção de acentos
   - Limpeza de HTML
   - Estruturação dos dados

3. **Feature Engineering**
   - Extração de palavras-chave
   - (Opcional) embeddings
   - (Opcional) score de relevância

4. **Armazenamento intermediário**
   - SQLite (uso interno no pipeline)

5. **Exportação final**
   - JSON otimizado para frontend

---

## 🌐 Frontend (Estático)

Hospedado via GitHub Pages.

### Responsabilidades:

- Carregar dados (`artigos.json`)
- Realizar buscas client-side
- Aplicar filtros:
  - ano
  - fonte
- Exibir resultados

---

# 💾 Estrutura de Dados

## Formato final (JSON)

```json
[
  {
    "id": 1,
    "titulo": "Exemplo de artigo",
    "resumo": "Resumo do artigo...",
    "ano": 2021,
    "fonte": "scielo",
    "keywords": ["educação", "homeschooling"]
  }
]
```

# ⚠️ Boas práticas

- Evitar textos muito longos
- Remover HTML bruto
- Manter JSON leve (< 50MB idealmente)

# ⚙️ Automação com GitHub Actions

O pipeline roda automaticamente (ex: mensalmente).

Exemplo de agendamento:

```yaml
on:
  schedule:
    - cron: '0 0 1 * *'  # todo dia 1 do mês
```

## O que o workflow faz:

- Executa scripts Python
- Atualiza o JSON
- Commita automaticamente no repositório
- Atualiza o GitHub Pages

# 📁 Estrutura do Projeto

```txt
project/
│
├── data_pipeline/
│   ├── scrape.py
│   ├── process.py
│   ├── export.py
│
├── data/
│   └── artigos.json
│
├── frontend/
│   ├── index.html
│   ├── app.js
│   └── style.css
│
├── .github/workflows/
│   └── update_data.yml
│
└── README.md
```

# 🔍 Funcionalidades (MVP → Avançado)

## ✅ MVP (fase inicial)

- Scraping básico  
- JSON consolidado  
- Busca por palavra-chave  
- Filtro por fonte/ano  

---

## 🚀 Intermediário

- Ranking simples (TF-IDF ou heurístico)  
- Deduplicação de artigos  
- Melhorias de busca:
  - case insensitive  
  - remoção de acentos  

---

## 🧠 Avançado

- Busca semântica (embeddings)  
- Clusterização de artigos  
- Sugestão de temas  
- Dashboard analítico  

---

# ⚠️ Limitações e decisões técnicas

## ❌ Sem backend

- GitHub Pages não suporta execução de código  
- Toda lógica deve ser:
  - pré-processada  
  - ou executada no browser  

---

## ❌ Sem LLM em tempo real

- Modelos exigem backend ou API paga  
- Solução:
  - pré-processamento offline  

---

## ⚠️ Performance

- Muitos artigos podem deixar o frontend lento  

**Soluções:**
- dividir JSON  
- usar indexação leve (ex: lunr.js)  

---

# 🧭 Roadmap

## 🔹 Fase 1

- [ ] Scraping de 1 repositório  
- [ ] Gerar JSON básico  
- [ ] Frontend simples  

---

## 🔹 Fase 2

- [ ] Integrar múltiplas fontes  
- [ ] Normalização de texto  
- [ ] Busca melhorada  

---

## 🔹 Fase 3

- [ ] Automação com GitHub Actions  
- [ ] Deploy no GitHub Pages  

---

## 🔹 Fase 4

- [ ] Ranking de relevância  
- [ ] Deduplicação  

---

## 🔹 Fase 5 (opcional)

- [ ] Embeddings  
- [ ] Busca semântica  
- [ ] Analytics  

---

# 🧩 Stack Tecnológica

## Backend (pipeline)

- Python  
- requests / BeautifulSoup  
- pandas  
- SQLite (uso interno)  

---

## Frontend

- HTML / CSS / JavaScript  
- (opcional) lunr.js  

---

## Infra

- GitHub Actions  
- GitHub Pages  

---

# 💡 Filosofia do Projeto

Este projeto segue três princípios:

### 1. Zero manutenção
Nada deve depender de serviços rodando continuamente.

### 2. Reprodutibilidade
Todo o pipeline deve ser versionado e automatizado.

### 3. Simplicidade
Preferir soluções estáticas e previsíveis ao invés de arquiteturas complexas.

---

# 📌 Próximos Passos

1. Implementar scraping inicial (SciELO recomendado)  
2. Criar pipeline básico em Python  
3. Gerar primeiro JSON  
4. Criar frontend simples  
5. Configurar GitHub Actions  

---

# 🧠 Observação Final

Este projeto pode evoluir facilmente para algo mais sofisticado, como:

- sistema de recuperação de informação (IR)  
- motor de busca semântico  
- ferramenta de apoio à pesquisa acadêmica  

Mas o foco inicial é claro:

> **funcionar bem, sozinho, e sem dor de cabeça.**