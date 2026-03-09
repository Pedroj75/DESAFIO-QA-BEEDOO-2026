# DESAFIO-QA-BEEDOO-2026

## Análise da Aplicação
**Aplicação testada:** [https://creative-sherbet-a51eac.netlify.app/](https://creative-sherbet-a51eac.netlify.app/)
### Objetivo da aplicação
A aplicação consiste em uma plataforma de gerenciamento de cursos, permitindo o cadastro e a listagem de cursos com informações como nome, descrição, modalidade (presencial ou online), datas de início e fim, número de vagas e imagem de capa.

### Principais fluxos disponíveis
- **Cadastro de curso:** formulário com campos de nome, descrição, modalidade, datas, vagas e imagem
- **Listagem de cursos:** exibição dos cursos cadastrados em formato de cards com suas informações
- **Exclusão de curso:** remoção de um curso da listagem

### Pontos mais críticos para teste
- Validação dos campos do formulário de cadastro
- Comportamento dos campos condicionais (endereço para presencial, link para online)
- Consistência das datas (início e fim)
- Exibição correta das informações nos cards da listagem
- Fluxo de exclusão e atualização da listagem após remoção

---

## Decisões Tomadas para Criação dos Testes

### Abordagem
Os testes foram estruturados em formato Gherkin (Dado/Quando/Então) para maior clareza e rastreabilidade entre cenário, execução e resultado.

### Critérios de priorização
- Fluxo principal de cadastro foi testado primeiro (caminho feliz)
- Em seguida, cenários negativos e validações de campos
- Por último, comportamentos inesperados

### Limitações identificadas durante a análise
- **Listagem vazia:** não foi possível testar o estado vazio da listagem pois o bug B06 impede a exclusão correta dos cursos cadastrados
- **Campos de data:** utilizam um componente de calendário que restringe a entrada a números e limita o ano ao máximo suportado pelo input nativo do navegador (275760). Valores acima desse limite são automaticamente corrigidos pelo componente, sendo este o ano máximo possível de ser cadastrado e o cenário testado em CT12.
- **Validações de campos obrigatórios:** o sistema não impede o cadastro com campos vazios, o que foi registrado como bug (B02)

---

## Raciocínio Durante a Análise

A exploração inicial da aplicação foi feita sem roteiro fixo, mapeando os campos disponíveis, os comportamentos esperados e os limites de cada funcionalidade. A partir disso, os cenários foram criados cobrindo:

- **Cenários positivos:** cadastro com dados válidos, exibição correta no card, campos condicionais funcionando
- **Cenários negativos:** valores inválidos, campos vazios, datas inconsistentes, valores fora do esperado
- **Comportamentos inesperados:** duplicidade de cadastro, textos excessivamente longos, ano inválido no campo de data

Durante a execução, alguns comportamentos inesperados foram identificados além dos cenários planejados, como notificações de exclusão duplicadas (B07) e desalinhamento visual nos cards (B08), que foram registrados como bugs adicionais.

---

## Casos de Teste

Os cenários e casos de teste foram documentados em uma planilha do Google Sheets:

[📋 Acessar planilha de casos de teste](https://docs.google.com/spreadsheets/d/1iRsnJi3UZc_Gq_RBTrZWDmZvIMZm-Ki_WaxNCFtCKPI/edit?usp=sharing)

---

## Evidências de Execução

As evidências de execução (prints e gravações de tela) estão organizadas por caso de teste e disponíveis no Google Drive:

[📁 Acessar pasta de evidências](https://drive.google.com/drive/folders/1Tl0DQEeoBvvhvdQlijl1Q2rqwTn-2jAc?usp=sharing)

### Estrutura da pasta
```
📁 Evidências
├── 📁 Testes
│   ├── 📹 CT01-Cadastro de curso com dados válidos
│   ├── 📹 CT02-Cadastro sem URL de imagem
│   ├── 📹 CT03-Validação de campos obrigatórios
│   ├── 📹 CT04-Data fim menor que data inicio
│   ├── 📹 CT05-Número de vagas negativo
│   ├── 📹 CT06-Número de vagas igual a zero
│   ├── 📹 CT07-Exibição do campo endereço para curso presencial
│   ├── 📹 CT08-Exibição do campo link de inscrição para curso online
│   ├── 📹 CT09-Exclusão de curso cadastrado
│   ├── 📹 CT10-Cadastro de curso duplicado
│   ├── 📹 CT11-Nome do curso com texto excessivamente longo
│   └── 📹 CT12-Inserção de data inválida
└── 📁 Bugs
    ├── 📹 B07-Notificações de Exclusão Duplicadas
    └── 📹 B08-Desalinhamento de Layout nos Cards
    └── 📹 B12-Vulnerabilidade XSS no campo de nome

```

> Os demais bugs (B01 a B06 e B09 a B11) estão evidenciados nos vídeos dos casos de teste correspondentes, conforme coluna "Bug Relacionado" na planilha. 

---

## Bugs Encontrados

| ID | Título | Severidade |
|---|---|---|
| B01 | Ausência de imagem padrão quando URL não é informada | Baixa |
| B02 | Cadastro de curso sem preenchimento de campos | Alta |
| B03 | Aceitação de datas inconsistentes | Alta |
| B04 | Campo de vagas aceita valores negativos | Média |
| B05 | Número de vagas igual a zero | Média |
| B06 | Exclusão de curso não remove da listagem | Alta |
| B07 | Notificações de exclusão duplicadas | Média |
| B08 | Desalinhamento crítico de layout nos cards | Baixa |
| B09 | Cadastro de curso duplicado | Média |
| B10 | Quebra de layout com nomes de cursos longos | Baixa |
| B11 | Aceitação de ano inválido no campo de data | Alta |
| B12 | Vulnerabilidade XSS no campo de nome   | Alta |

O relatório completo com passos para reproduzir, resultado atual e resultado esperado está disponível na planilha linkada acima.

---

## Uso de Inteligência Artificial

Durante o desafio, utilizei o Claude (Anthropic) como ferramenta de apoio ao raciocínio. O uso foi feito de forma consciente e crítica, nas seguintes situações:

- **Revisão da cobertura de testes:** após estruturar os cenários, utilizei a IA para identificar possíveis lacunas. As sugestões foram avaliadas uma a uma e algumas foram aproveitadas, outras descartadas por não se aplicarem ao comportamento real da aplicação (ex: testes de filtro na listagem, que não existe na aplicação, e formato de data inválido via caracteres, que o campo não permite)
- **Estruturação de novos cenários:** cenários adicionais sugeridos pela IA foram adaptados ao formato Gherkin já utilizado na planilha
