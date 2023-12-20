# Objetivo

O objetivo deste projeto é configurar uma aplicação web utilizando o Strapi e o PostgreSQL e implantá-la em um cluster Kubernetes.

## Serviço

O serviço a ser implementado consistirá em uma aplicação web simples utilizando o Strapi como framework. O Strapi é uma plataforma de conteúdo headless que permite a criação de APIs e aplicações web baseadas em conteúdo. O PostgreSQL será utilizado como banco de dados para armazenar os dados da aplicação.

Para este fim, utilizaremos a aplicação Food Advisor disponível em [https://github.com/strapi/foodadvisor.git](https://github.com/strapi/foodadvisor.git), uma aplicação de demonstração implementada no Strapi. A abordagem envolve clonar este repositório, construir a imagem Docker, criar o Helm chart e, posteriormente, implantar a aplicação em um cluster Kubernetes.

## Recursos do Kubernetes

Os recursos necessários no Kubernetes para o Strapi incluem:

- Dois pods: um para a API e outro para o cliente
- Dois serviços: um para a API e outro para o cliente
- Um secret para armazenar as credenciais de conexão da API com o banco de dados
- Dois deployments: um para a API e outro para o cliente
- Dois ingress: um para a API e outro para o cliente

Para o PostgreSQL, será utilizado o chart pronto disponível no repositório da Bitnami: [https://github.com/bitnami/charts/tree/main/bitnami/postgresql](https://github.com/bitnami/charts/tree/main/bitnami/postgresql)

## Passo a Passo para Implementação do Projeto

### Preparação do Ambiente

1. Instale e inicie o Minikube:
   ```bash
   minikube start
   ```
2. Verifique se o Minikube está rodando corretamente:
    ```bash
    minikube status
    ```
3. Se esta for a primeira vez que você está utilizando o Ingress no Minikube, habilite o complemento:
    ```bash
    minikube addons enable ingress
    ```

### Implantação do Serviço

1. Clone este repositório:
   ```bash
   git clone https://github.com/MG10exe/Projeto-Helm-FoodAdvisor
   ```
2. Entre na pasta do Strapi Food Advisor:
    ```bash
    cd projetotal/strapi-foodadvisor
    ```
3. Atualize as dependências do Helm:
    ```bash
    helm dependency update
    ```
4. Retorne ao diretório raiz:
    ```bash
    cd ..
    ```
5. Instale o chart do Food Advisor:
    ```bash
    helm install foodadvisor-chart ./foodadvisor-chart
    ```
6. Verifique se a aplicação foi implantada com sucesso:
    ```bash
    kubectl get all
    ```

### Teste do Serviço

1. Para testar o serviço, execute os seguintes comandos:
   ```bash
   kubectl port-forward svc/ingress-nginx-controller 8889:80 -n ingress-nginx
   ```

    ```bash
    kubectl port-forward svc/foodadvisor-backend 1337:80
    ```
2. Acesse o endereço http://foodadvisor.backend:8889/ para verificar o funcionamento do Strapi e criar a conta de administrador.

3. Acesse http://localhost:1337/admin para criar a conta de administrador, informando os dados solicitados. Após isso, o painel do Strapi será exibido.
