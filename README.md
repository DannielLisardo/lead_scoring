# Modelo de Lead Scoring

Este projeto implementa um modelo de **lead scoring** para priorização de leads comerciais com base na probabilidade de conversão.

O objetivo é demonstrar uma abordagem completa de ciência de dados, incluindo engenharia de features, uso de NLP para dados textuais e treinamento de modelo de machine learning.

Os dados utilizados são **fictícios**, criados apenas para fins de demonstração.

---

# Objetivo do Projeto

Em operações comerciais com grande volume de leads, priorizar corretamente quais leads devem ser abordados primeiro pode aumentar significativamente a eficiência do time de vendas.

Este projeto busca construir um modelo que estime a **probabilidade de conversão de um lead**, permitindo priorizar aqueles com maior potencial.

---

# Arquitetura de Dados (Medallion)

A preparação dos dados seguiu o padrão **Medallion Architecture**, com separação em camadas:

- **Bronze**: ingestão dos dados brutos dos leads
- **Silver**: limpeza, padronização e enriquecimento dos dados
- **Gold**: dataset final consolidado, pronto para consumo pelo modelo

As principais etapas de tratamento e engenharia de features foram realizadas nas camadas **Silver e Gold**, incluindo:

- tratamento de valores inconsistentes e dados faltantes  
- padronização de variáveis categóricas  
- criação de variáveis temporais (hora do dia, dia da semana, mês)  
- cálculo de métricas históricas, como:
  - taxa de conversão por funil  
  - taxa de conversão por vendedor  
  - taxa de conversão por vendedor dentro do funil  
- agregações de volume de leads por contexto  

Essa separação garante maior organização, reprodutibilidade e alinhamento com práticas modernas de engenharia de dados.

---

# Dados Utilizados

O dataset inclui informações como:

- idade do lead
- canal de origem
- escolaridade
- renda estimada
- turno preferido para contato
- tempo de preenchimento da ficha
- funil de origem
- objetivo declarado pelo lead (texto livre)

Além disso, foram criadas **features derivadas**, como:

- hora do dia
- dia da semana
- mês
- taxa histórica de conversão por funil
- taxa histórica de conversão por vendedor
- volume de leads por funil

---

# Uso de NLP

A coluna **objetivo_texto** contém texto livre preenchido pelo lead.

Para extrair informação desse campo foi utilizada a técnica **TF-IDF (Term Frequency – Inverse Document Frequency)**, que transforma o texto em vetores numéricos utilizáveis pelo modelo.

---

# Modelo Utilizado

O modelo escolhido foi:

**Random Forest Classifier**

Principais motivos:

- boa performance em problemas tabulares
- robustez a diferentes tipos de features
- pouca necessidade de normalização dos dados
- facilidade de interpretação relativa das variáveis

---

# Métrica de Avaliação

A métrica utilizada foi:

**Precision@K**

Essa métrica mede a proporção de leads que realmente converteram entre os **K leads com maior score gerado pelo modelo**.

No contexto de vendas, essa métrica é mais relevante que acurácia, pois o objetivo não é classificar todos os leads corretamente, mas sim **priorizar os melhores leads para o time comercial**.

Resultado obtido no experimento:

Precision@100 ≈ 0.36

Ou seja, aproximadamente **36% dos 100 leads priorizados pelo modelo converteram**.

---

# Estrutura do Projeto

lead_scoring_project/

data/
    leads_sample.csv
    Dataset fictício utilizado para demonstração

src/
    train_lead_scoring.py
    Script de treinamento do modelo

    predict_lead_scoring.py
    Script para geração de scores de leads

models/
    lead_scoring_model.pkl
    Modelo treinado exportado

notebooks/
    exploration.ipynb
    Notebook exploratório opcional

requirements.txt
    Dependências do projeto

README.md
    Documentação do projeto

---

# Como Executar o Projeto

## 1. Instalar dependências

pip install -r requirements.txt

---

## 2. Treinar o modelo

python src/train_lead_scoring.py

Esse script:

- carrega os dados
- executa o pipeline de features
- treina o modelo
- salva o modelo treinado

---

## 3. Gerar scores de leads

python src/predict_lead_scoring.py

Esse script:

- carrega o modelo treinado
- calcula o score de conversão de cada lead
- retorna os leads com maior probabilidade de conversão

---

# Tecnologias Utilizadas

- Python
- Pandas
- Scikit-Learn
- MLflow (no treinamento original)
- TF-IDF para processamento de texto

---

# Observações

Este projeto foi desenvolvido utilizando **dados sintéticos**, com o objetivo de demonstrar a abordagem de modelagem e priorização de leads.

Em um cenário real, etapas adicionais seriam recomendadas, como:

- validação temporal mais rigorosa
- monitoramento de drift do modelo
- re-treinamento periódico
- integração com sistemas de CRM

---

# Autor

Danniel Lisardo
