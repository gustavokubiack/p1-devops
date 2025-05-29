# 🛠️ p1-devops

Este projeto tem como objetivo demonstrar o ciclo completo de uma aplicação Python containerizada com Kubernetes, utilizando [Kind](https://kind.sigs.k8s.io/) para criação de clusters locais e [ArgoCD](https://argo-cd.readthedocs.io/) para entrega contínua automatizada.

---

---

## 🧾 Pré-requisitos

* Python 3.8+
* [Docker](https://docs.docker.com/get-docker/)
* [Kind](https://kind.sigs.k8s.io/docs/user/quick-start/)
* [kubectl](https://kubernetes.io/docs/tasks/tools/)

---

## 🚀 Clonando o Repositório

```bash
git clone https://github.com/gustavokubiack/p1-devops.git
cd p1-devops
```

---

## 🐍 Criando Ambiente Virtual no Linux

```bash
python3 -m venv venv
source venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt
```

---

## ⚙️ Criando o Cluster com Kind

```bash
kind create cluster --config kind.yaml --name p1-devops
```

---

## 🐙 Build e Push da Imagem Docker

Altere a `tag` se necessário:

```bash
docker build -t gustavokubiack/p1-devops:latest .
docker push gustavokubiack/p1-devops:latest
```
- **Necessário ter configurado o docker com seu usuário pessoal**
---

## 📦 Aplicando os Manifests Kubernetes

```bash
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
```

Verifique se tudo está rodando:

```bash
kubectl get po
kubectl get svc
```

---

## 🔍 Instalando e Configurando ArgoCD

### Instale o ArgoCD:

```bash
kubectl create namespace argocd

kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

### Exponha a interface do ArgoCD:

```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

Acesse: [https://localhost:8080](https://localhost:8080)

Credenciais iniciais:

* **Usuário:** `admin`
* **Senha:** Execute:

  ```bash
  kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
  ```

  O retorno do comando acima será a senha para logar no ArgoCD.
---

* **Necessário configurar a aplicação no ArgoCD utilizando o [tutorial](https://eduardo-da-silva.github.io/fundamentos-devops/cd/#configuracao-do-argocd)**

## ✅ Testando

Para rodar os testes:

```bash
pytest
```

---

## 🛠️ Alterações Realizadas no Código

Durante o desenvolvimento do projeto, algumas modificações foram feitas para garantir o correto funcionamento da aplicação e demonstrar sua execução completa. Abaixo estão as principais mudanças:

### 📁 `app/main.py`

  **Execução com Uvicorn**
   Foi adicionado o bloco condicional para executar a API utilizando o `uvicorn`, permitindo iniciar o servidor diretamente com `python app/main.py`:



  **Correção do Endpoint `/square/x`**
   O endpoint responsável por calcular o quadrado de um número estava incorretamente **somando dois termos** em vez de **elevar ao quadrado**. Foi ajustado para realizar a operação correta:


---

### 📁 `tests/test_main.py`

* **Ajuste no Retorno da Mensagem Padrão**
  O teste da rota principal `/` precisou ser ajustado para refletir a nova mensagem retornada, a qual foi alterada com o objetivo de **demonstrar corretamente ao professor** o funcionamento.
