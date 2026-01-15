Sistema de Migração de Prontuários (Word to Django)
Este repositório contém as ferramentas necessárias para extrair dados de prontuários em formato .docx (Microsoft Word)
e importá-los automaticamente para um sistema de gestão de fisioterapia desenvolvido em Django.

Funcionalidades
. Extração Automatizada: Lê múltiplos arquivos Word em uma pasta e identifica blocos de dados (Cadastro, Anamnese e Evoluções)
. Limpeza de Dados: Trata automaticamente campos obrigatórios (como Peso e Data de Nascimento) para evitar erros de integridade no banco de dados
. Formatação ISO: Converte datas do formato brasileiro (DD/MM/AAAA) para o padrão de banco de dados (AAAA-MM-DD)
. Mapeamento Inteligente: Vincula evoluções e anamneses aos pacientes correspondentes através do nome.

 Estrutura dos Arquivos de Migração
Arquivo                     Descrição
convertetudo.py             Script Python que processa os arquivos .docx e gera os 3 CSVs finais.
pacientes_final.csv         Dados cadastrais (Nome, CPF, Nascimento, Peso, Fisioterapeuta responsável).
dados_paciente_final.csv    Dados clínicos/Anamnese (QP, HMA, HPP, Exame Físico, etc.).
evolucoes_final.csv         Histórico de atendimentos datados com títulos e descrições.

 Requisitos
 - Python 3.xBibliotecas Python: python-docx, pandas
 - Django App com a biblioteca django-import-export instalada.
 
-  Como Usar
  1. Preparação dos DadosColoque todos os arquivos .docx na pasta configurada no script (ex: /mnt/d/python nutry-lab/evolucoesvanessa).
  2. Geração dos CSVsExecute o script de conversão:
     Bash:
       python3 convertetudo.py
  3. Importação no Django Admin
     Acesse o painel administrativo do Django e realize a importação na seguinte ordem obrigatória:
       Pacientes: Importe pacientes_final.csv.
       Dados do Paciente: Importe dados_paciente_final.csv.
       Evoluções: Importe evolucoes_final.csv.
   Nota: A ordem é importante para que o Django consiga criar as chaves estrangeiras (vínculos) corretamente entre os modelos.

Observações Técnicas
     Campos de Texto: Foi configurado um limite de 200 caracteres no script para coincidir com os campos CharField do banco de dados.
       Para textos maiores, recomenda-se alterar os modelos para TextField.
     Dados Ausentes: O sistema preenche automaticamente datas inexistentes como 1900-01-01
       e o peso como 01 para satisfazer as restrições NOT NULL do banco de dados.
