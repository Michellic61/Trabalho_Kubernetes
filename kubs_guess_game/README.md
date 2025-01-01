# Trabalho Prático Unidade 2 - Kubernetes




# Jogo de Adivinhação com Flask usando Kubernetes






## Funcionalidades





- **Criação de um novo jogo**: O usuário pode iniciar um novo jogo fornecendo uma senha.

- **Adivinhação da senha**: O jogador tenta adivinhar a senha e recebe feedback sobre as letras corretas e suas posições.

- **Armazenamento seguro**: As senhas são armazenadas utilizando base64.

- **Dicas**: Adivinhações incorretas retornam mensagens com dicas.





## Requisitos

- **Kubernetes**
- **Docker**
- **Git**





## Conceitos de Kubernetes Aplicados



### 1. Pods


- **Definição**: A menor unidade de implantação em Kubernetes, um pod pode conter um ou mais contêineres que compartilham recursos de rede e armazenamento.

- **Aplicação**: No jogo de adivinhação, cada componente (backend, frontend) é executado em pods separados, garantindo isolamento e gerenciamento eficiente.



### 2. Deployments


- **Definição**: Gerenciam a criação e atualização de pods. Permitem definir o número de réplicas e garantem que o estado desejado da aplicação seja mantido.

- **Aplicação**: Utilizado para gerenciar o backend e o frontend do jogo, garantindo que sempre haja um número específico de instâncias em execução.



### 3. Services


- **Definição**: Abstraem o acesso aos pods, fornecendo um ponto de entrada estável para a comunicação entre diferentes partes da aplicação.

- **Aplicação**: O serviço frontend-svc expõe o frontend do jogo para acesso externo via NodePort.



### 4. ConfigMaps e Secrets


- **Definição**: ConfigMaps armazenam dados de configuração não sensíveis, enquanto Secrets armazenam dados sensíveis, como senhas.

- **Aplicação**: ConfigMaps são usados para armazenar configurações do NGINX, e Secrets para armazenar credenciais do banco de dados.



### 5. Persistent Volumes (PV) e Persistent Volume Claims (PVC)


- **Definição**: PVs são recursos de armazenamento provisionados pelo administrador, enquanto PVCs são solicitações de armazenamento feitas pelos usuários.

- **Aplicação**: Utilizados para armazenar dados persistentes do banco de dados PostgreSQL, garantindo que os dados não sejam perdidos mesmo se os pods forem reiniciados.



### 6. Horizontal Pod Autoscaler (HPA)


- **Definição**: Ajusta automaticamente o número de pods com base na utilização de recursos, como CPU.

- **Aplicação**: Configurado para o backend do jogo, garantindo que a aplicação possa escalar horizontalmente conforme a demanda, mantendo o desempenho.



### 7. Ingress


- **Definição**: Gerencia o acesso externo aos serviços dentro do cluster, geralmente usando um controlador de ingress, como NGINX.

- **Aplicação**: Utilizado para rotear o tráfego para o frontend e backend do jogo, facilitando o acesso dos usuários.





## Instalação e Execução

1.**Clonar o repositório**: 

   ```bash
    git clone https://github.com/Michellic61/Trabalho_Kubernetes.git

   ```

1. **Navegar até a pasta**:

   ```bash
   cd kubs_guess_game

   ```


2. **Subir a aplicação**:

   ```bash
   kubectl apply -f kubs_guess_game/ -R

   ```


3. **Verificar o status dos pods**:

   ```bash
   kubectl get pods

   ```


   Aguarde até que todos os pods estejam com o status 'Running'.



4. **Acessar a aplicação**:

   Abra o navegador e vá para a URL http://localhost:30080 (NodePort).

   Para encontrar a porta do serviço frontend-svc (NodePort), use:

   ```bash
   kubectl get svc

   ```




5. **Verificar o status dos pods**:

```bash
kubectl get pods

```

Aguarde até que todos os pods estejam com o status 'Running'.



6. **Acessar a aplicação**: 
Abra o navegador e vá para a URL:

```bash
http://localhost:30080 (NodePort)

```

Para encontrar a porta do serviço frontend-svc (NodePort), use:

```bash
kubectl get svc

```




## Como Jogar


1. **Criar jogo**:

. Acesse a URL acima

. Digite uma frase secreta

. Envie e salve o game-id


2. **Adivinhar a senha**:

. Vá para o endponint breaker

. Entre com o game_id que foi gerado pelo Creator

. Tente adivinhar





## Estrutura do Código

Repositório: Contém todos os arquivos necessários para subir a aplicação.

- **Arquivos YAML**: Instruções para deploy, incluindo ConfigMaps, PV e PVC, Serviços, Secrets, e HorizontalPodAutoscaler.

- **HorizontalPodAutoscaler**: Configurado para autoscaling do backend com base nas métricas de CPU, com mínimo de 1 e máximo de 6 réplicas.

- **Ingress**: Utiliza NGINX como Ingress Controller para frontend e backend.

- **Configuração do NGINX**: Ajustada dentro do frontend.yaml no ConfigMap.






## Melhorias Implementadas

- Imagens Docker: Utilização de imagens disponíveis no DockerHub

- Pull de Imagem: Realizado quando a imagem não está presente.

- StatefulSet: Utilizado para o banco de dados Postgres.

- HPA: Balanceamento de carga no backend.

- Reinício de Container: Implementado com restart: always.

- Volume Persistente: Para o banco de dados PostgreSQL utilizando PV e PVC.





## Atualizações de Código


Para aplicar alterações no código:

```bash
kubectl apply -f .

```

Para finalizar o processo:

```bash
kubectl delete -f .

```