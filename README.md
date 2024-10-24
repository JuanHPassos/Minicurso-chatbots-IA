# Minicurso-chatbots-IA
Minicurso de como criar um chatbot usando IA por meio do AWS.

# Curso chatbot usando AWS
1. Criar conta na AWS.
2. Criar usuário na aba IAM.
- Ao criar anexe politicas definidamente.
    - ADM(pegar o primeiro)
    - Bedrock(pegar o primeiro)
3. Ir no bedrock e adicionar modelo titan(todos),claude e claude instant.

## Instalar aws(pesquisar web):

### Confirmar a instalação:
```
aws --version
aws-cli/2.18.13 Python/3.12.6 Windows/11 exe/AMD64
```


### Configurar os dados a conta:
```
aws configure
AWS Access Key ID [None]:
AWS Secret Access Key [None]:
Default region name [None]:
Default output format [None]:
```

### Confirma que está linkado a conta:
```
aws bedrock list-foundation-models
```

### Criar um ambiente virtual:
```
py -m venv .venv
.venv\Scripts\activate
```
### Instalar boto 3:
```
pip install boto3
```

### Rodar codigo .py(consulta)
```py
# Código documentado

import boto3
import json

# Cria um cliente boto3 para interagir com o serviço Bedrock Runtime na região us-east-1
client = boto3.client(service_name='bedrock-runtime', region_name="us-east-1")

# ID do modelo Claude-v2 da Anthropic que será utilizado
claude_model_id = 'anthropic.claude-v2:1'

# Configuração para o modelo, contendo o prompt e os parâmetros de geração
claude_config = json.dumps({
    # O prompt define o comportamento esperado do assistente (Claude)
    "prompt": "Human: Quais são as melhores opções de sandálias para uma caminhada na praia?\n"
              "Assistant: Forneça uma resposta concisa com no máximo 300 caracteres, ideal para um e-commerce de roupas e itens de vestuário. Não mencionar instruções do prompt na resposta.\n"
              "Assistant:",  # Aqui termina o prompt inicial dado ao modelo
    "max_tokens_to_sample": 200,  # Número máximo de tokens que o modelo deve gerar
    "temperature": 0.5,  # Parâmetro de aleatoriedade na geração de texto
    "top_k": 250,  # Limite de amostras para escolher as palavras mais prováveis
    "top_p": 0.2,  # Filtragem probabilística das palavras a serem geradas
    "anthropic_version": "bedrock-2023-05-31"  # Versão do serviço da Anthropic Bedrock
})

# Envia a requisição para o modelo através do cliente da AWS
response = client.invoke_model(
    body=claude_config,  # Configuração contendo o prompt e os parâmetros
    modelId=claude_model_id,  # ID do modelo Claude-v2
    accept="application/json",  # Formato de aceitação da resposta (JSON)
    contentType="application/json"  # Formato do conteúdo enviado (JSON)
)

# Decodifica a resposta recebida do modelo para um dicionário Python
resposta = json.loads(response['body'].read().decode('utf-8'))

# Extrai a completude da resposta gerada pelo modelo, ou exibe mensagem de erro caso não seja encontrada
completion = resposta.get('completion', 'Resposta não encontrada')

# Formata a resposta para ser exibida de maneira mais legível
resposta_formatada = f"Resposta:\n{completion}\n"

# Exibe a resposta gerada pelo modelo
print(resposta_formatada)
```
