# 🌐 Project: GitOps Static Web (GCP DevOps Edition)

Este projeto tem como objetivo construir uma infraestrutura de hospedagem de site estático segura e totalmente automatizada no Google Cloud Platform (GCP). A ideia é integrar conceitos de **Infraestrutura como Código (IaC)** com **CI/CD baseado em GitOps** para automatizar o deploy de infraestrutura e do frontend sem o uso de credenciais estáticas.

## 🏗️ Arquitetura
1. **Terraform**: Provisionamento de Cloud Storage, Cloud CDN, Global HTTP(S) Load Balancer e SSL Certificates.
2. **GitHub Actions**: Pipeline de CI/CD para validação do Terraform e deploy dos assets do frontend.
3. **Workload Identity Federation**: Autenticação sem chaves baseada em OIDC (OpenID Connect) entre GitHub e GCP.

---

## 🎯 Objetivos do Desafio

### Fase 1: Fundação & Autenticação (Segurança)
- [ ] Configurar o repositório Git com a estrutura de pastas separando IaC de assets do frontend.
- [ ] Criar um Bucket no Cloud Storage para o armazenamento remoto do estado do Terraform (`default.tfstate`) com Object Versioning ativo.
- [ ] Implementar o **Workload Identity Federation (WIF)** no GCP para estabelecer a relação de confiança OIDC com o GitHub.
- [ ] Criar a Service Account no GCP com permissões estritas de privilégio mínimo para o runner do GitHub.
- [ ] Escrever o workflow base do GitHub Actions que realiza o handshake de autenticação e obtém o token de acesso temporário.

### Fase 2: Infraestrutura como Código (IaC)
- [ ] Configurar o arquivo `providers.tf` com as versões necessárias e o bloco do backend remoto.
- [ ] Criar um recurso de **Cloud Storage Bucket** privado configurado para static web hosting (`index.html`).
- [ ] Configurar o recurso de **Backend Bucket** no Terraform ativando o **Cloud CDN** para cache de borda.
- [ ] Provisionar o **Global HTTP(S) Load Balancer** definindo URL maps, proxies de destino e as regras de encaminhamento.
- [ ] Implementar o recurso de **Google-managed SSL Certificate** para provisionamento automático do TLS.

### Fase 3: Automação & GitOps Core
- [ ] Estender a pipeline do GitHub Actions para rodar automações de lint, validação (`terraform fmt`, `terraform validate`) e planejamento.
- [ ] Configurar a pipeline para injetar o resultado do `terraform plan` diretamente como comentário em Pull Requests.
- [ ] Implementar a etapa de sincronização dos arquivos do frontend utilizando o comando `gcloud storage rsync`.
- [ ] Adicionar um passo automatizado para invalidação de cache no Cloud CDN (`gcloud compute url-maps invalidate-cdn-cache`) após o deploy do frontend.

### Fase 4: Observabilidade & Validação
- [ ] Validar a propagação do DNS e o mapeamento correto do domínio para o IP do Load Balancer.
- [ ] Testar a terminação TLS e verificar o cabeçalho de cache nas ferramentas de desenvolvedor do navegador.
- [ ] Configurar buckets de **Cloud Logging** e métricas para monitorar o tráfego de entrada e a taxa de cache hit/miss do CDN.
- [ ] Garantir o travamento de estado (state locking) nativo do GCP para prevenir concorrência de execuções.

---

## 📚 Documentação de Apoio Principal

- **Google Cloud Terraform Provider:** [registry.terraform.io/providers/hashicorp/google](https://registry.terraform.io/providers/hashicorp/google/latest/docs)
- **GCP Workload Identity Federation Guide:** [cloud.google.com/iam/docs/workload-identity-federation](https://cloud.google.com/iam/docs/workload-identity-federation)
- **Cloud Storage Static Website Hosting:** [cloud.google.com/storage/docs/hosting-static-website](https://cloud.google.com/storage/docs/hosting-static-website)
- **GitHub Actions Configuration for GCP:** [github.com/google-github-actions/auth](https://github.com/google-github-actions/auth)

---

## 🛠️ Como executar
1. Inicializar o Terraform: `terraform init`
2. Validar a sintaxe: `terraform validate`
3. Planejar as mudanças: `terraform plan`
4. Aplicar a infraestrutura: `terraform apply`
