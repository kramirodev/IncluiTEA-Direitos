# IncluiTEA-Direitos — Assistente de Rastreio e Requerimento de Direitos Sociais e Terapias no SUS

## 1. Identificação

**Título do MVP:** IncluiTEA-Direitos — Assistente de Rastreio e Requerimento de Direitos Sociais e Terapias no SUS

**Título do artigo científico:** Acesso a Terapias e Judicialização da Saúde: um instrumento digital de apoio a mães e cuidadores de crianças autistas em áreas periféricas

**Revista indicada:** Pensar Acadêmico (UNIFACIG) ou Revista Eletrônica Direito e Sociedade (REDIN/FACCAT), por trabalharem com direitos humanos, inclusão social e judicialização do acesso à saúde.

## 2. Problema e justificativa

Famílias de baixa renda, especialmente em áreas periféricas, enfrentam dificuldades para acessar terapias e benefícios garantidos por lei. O obstáculo raramente é apenas a ausência do direito: está na falta de informação sobre quais documentos são exigidos, em qual órgão protocolar, quais prazos acompanhar e o que fazer diante de um indeferimento. Como consequência, laudos se perdem, exigências são cumpridas de forma incompleta e o acesso acaba dependendo da judicialização.

## 3. Solução proposta

O IncluiTEA-Direitos é um assistente passo a passo que orienta mães e cuidadores na organização dos documentos necessários para requerer:

- terapias no SUS (ABA, fonoaudiologia, terapia ocupacional);
- transporte gratuito (Passe Livre);
- Benefício de Prestação Continuada (BPC).

Ao final do preenchimento, o sistema gera um checklist personalizado por benefício e declarações em PDF, informando o que já está pronto, o que falta e a quais órgãos os documentos se destinam.

## 4. Simplicidade e arquitetura

A solução adota um frontend estático, organizado como wizard (formulário guiado por etapas). Não há servidor nem banco de dados: o progresso é salvo no próprio dispositivo, por meio do armazenamento local do navegador, e os PDFs são gerados no cliente. Essa escolha reduz custo de implantação, dispensa autenticação e mantém os dados sensíveis das famílias sob sua própria guarda — aspecto relevante quando se trata de informações de saúde de crianças.

## 5. Modelo de classes

O diagrama de classes do MVP possui 20 classes, distribuídas em cinco grupos:

| Grupo | Classes |
|---|---|
| Apresentação (wizard) | SessaoWizard, EtapaWizard, Pergunta, RespostaFormulario |
| Serviços | RepositorioLocal, GeradorPDF, ServicoElegibilidade |
| Domínio familiar | Responsavel, Familia, Crianca |
| Documentação | Documento («abstract»), LaudoMedico, ComprovanteResidencia, ComprovanteCadUnico |
| Direitos | Beneficio, CriterioElegibilidade, Requerimento, Checklist, ItemChecklist, Declaracao |

A classe **SessaoWizard** representa a jornada de preenchimento: registra o identificador da sessão, a data de início, a etapa atual e se o preenchimento foi concluído, além de permitir avançar, voltar e gerar o checklist. Ela existe separadamente porque, sem login, é o que permite salvar e retomar o atendimento, e porque as respostas do formulário precisam estar vinculadas a um mesmo preenchimento.

As relações centrais do modelo são: uma **Familia** agrega responsáveis e crianças; a **Crianca** anexa **Documentos** e abre **Requerimentos**; cada **Requerimento** está vinculado a um **Beneficio**, que é composto por critérios de elegibilidade; o requerimento gera um **Checklist** formado por itens, e cada item pode apontar para o documento que serve de evidência. A classe **Documento** é abstrata e se especializa em laudo médico, comprovante de residência e comprovante do CadÚnico — os três tipos exigidos com maior frequência pelos órgãos. O **GeradorPDF** produz a declaração e o checklist, e o **ServicoElegibilidade** verifica, a partir dos dados da família, se os critérios de cada benefício estão atendidos.

## 6. Fluxo de uso representado no diagrama

O usuário inicia uma **SessaoWizard**, percorre as etapas, responde às perguntas e anexa os documentos. As respostas originam um **Requerimento**, ligado a uma criança e a um benefício. O sistema avalia a elegibilidade, monta o **Checklist** com os documentos pendentes e gera as declarações em PDF. Se o requerimento for indeferido, o próprio assistente orienta sobre o recurso administrativo e o encaminhamento à Defensoria Pública ou ao Ministério Público — o percurso da judicialização.

## 7. Resultados esperados

Espera-se reduzir idas improdutivas a órgãos públicos, diminuir a perda de prazos e tornar compreensível o caminho entre o laudo e a terapia. O instrumento não substitui o atendimento jurídico nem o serviço social, mas organiza a informação e a documentação que sustentam esse atendimento, contribuindo para que o acesso a direitos deixe de depender exclusivamente do conhecimento prévio de cada família.
