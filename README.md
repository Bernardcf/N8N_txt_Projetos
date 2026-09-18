# N8N_txt_Projetos

Atue como um especialista em N8N.

Crie uma automação para automatizar no N8N requisições de aprovação em um processo de compra ou manutenção de ferramentas de IA porque eu fiz umas 200 solicitações desse tipo no mês e mal passou do dia 15.

Público:
Equipe de TI

Ferramentas envolvidas:
Google Sheets e Freshservice.

Fluxo:
1. Receber uma nova resposta de um formulário específico com caixas selecionáveis do Freshservice como gatilho webhook Ticket: Retrieve.
2. Verificar se o número do chamado é novo e se o Agente do chamado é da equipe de TI com a função Pull Agent.
3. Mudar o status do chamado para "Pending" nó do Freshservice Ticket: Update pausando o SLA.
4. Normalizar os dados preenchidos com um nó Edit Fields (Set) ou Code para criar um objeto padronizado e comparar os campos com planilhas no Google Sheets com os custos fixos e variáveis dos créditos nas principais ferramentas de IA utilizadas pela empresa, o nome dos gestores pra aprovação e os dados do projeto com o nó próprio dele.
5. Consolidar os resultados dos campos com um Merge no modo Append.
6. Com base em uma regra de decisão IF ou um Switch (com condições especiais), enviar sequencialmente, com um nó HTTP Request em uma função de loop com uma pausa para não repetir, os pedidos de aprovação no Freshservice para o e-mail dos gestores. Usar um nó no meio para checar se Request Approval = Approved, e passar para a próxima aprovação.
7. Executar uma ação Ticket: Update do Freshservice modificando o status do chamado para "Awaiting Approval" (ou "Cancelled" se fugir do escopo ou for negado).
8. Postar com um nó usando a função Ticket: Comment do Freshservice uma mensagem dizendo "Aguardando autorização. O pedido segue para a aprovação do gestores @gestor1 e @gestor2"  no chamado com as variáveis substituindo os nomes dos envolvidos. Se for cancelado ou aguardar retorno, postar o "motivo_decisao": "Valor acima do limite estabelecido de budget_projeto", por exemplo.

Regras:
Usar os nodes integrados do Freshservice.
Autenticar com a API Key da aplicação, o subdomínio da conta Freshservice.
O nó nativo do Freshservice no n8n suporta consultar e atualizar tickets, mas não apresenta uma operação específica para criar aprovações. Operações adicionais devem ser realizadas com o nó HTTP Request usando a API do Freshservice.
Ao Normalizar tem que Remover espaços, Converter código para maiúsculas, Converter do valores para números, padronizar respostas True/False Sim/Não e Validar campos obrigatórios.
Utilizar parâmetros para os campos no fluxo advindo do Freshservice, por exemplo codigo/setor/gestor/agente/limite_aprovacao/status.
Utilizar parâmetros para os campos no fluxo do n8n, por exemplor esultado_validacao_n8n/aprovacao_necessaria_n8n/gestor_verificado_n8n/decisao_n8n/processado_n8n/id_execucao_n8n.
Em cada nó das planilhas deve ser especificado o recurso Sheet Within Document e fazer operações do tipo Get Row(s) e filtros como Centro_custo para resolver a posição exata dos dados, fileira e coluna. Tem que marcar a opção Always Output Data e adicionar um Edit Fields para identificar a origem True depois de cada consulta, evitando o travamento caso não ache nada.
Verificar se os campos preenchidos referentes à identificação do projeto são de um projeto registrado em planilhas de BI e Controladoria. Disparar um comentário no chamado solicitando mais informações se faltarem as informações pertinentes como Centro de Custo.
Verificar que a pessoa designada no formulário exerce uma função de gestor em uma planilha do RH.
Ignorar registros sem e-mail válido ou .com.br. Deixar apenas e-mails com final .com.

Resultado esperado:
Enviar uma mensagem requisitando a autorização do gestor com a cotação do produto ou serviço de IA mapeado com base no pedido original.




Explique quais nós do N8N devem ser utilizados e a lógica de funcionamento do workflow.
