# 🔐 Guia de Configuração SSO SAML
### Integração Microsoft Entra ID (Azure AD) ↔ INDECX

[![Microsoft Entra ID](https://img.shields.io/badge/Microsoft%20Entra%20ID-0078D4?style=flat&logo=microsoft&logoColor=white)](https://portal.azure.com)
[![SAML 2.0](https://img.shields.io/badge/SAML%202.0-FF6B35?style=flat&logo=xml&logoColor=white)](https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=security)
[![SSO](https://img.shields.io/badge/Single%20Sign--On-28A745?style=flat&logo=key&logoColor=white)](https://en.wikipedia.org/wiki/Single_sign-on)

---

## 📋 Índice

- [Visão Geral](#-visão-geral)
- [Pré-requisitos](#-pré-requisitos)
- [Configuração Passo a Passo](#-configuração-passo-a-passo)
  - [1. Criar Enterprise Application](#1-criar-enterprise-application-saml-no-azure-ad)
  - [2. Configurar SSO SAML](#2-configurar-single-sign-on-sso-via-saml)
  - [3. Obter Dados para INDECX](#3-obter-as-informações-dados-mycompany-para-configurar-na-indecx)
  - [4. Compartilhar com INDECX](#4-compartilhar-com-a-equipe-indecx-e-configuração-no-lado-indecx)
- [Documentação Microsoft](#-documentação-e-referências-microsoft)
- [Troubleshooting](#-troubleshooting)

---

## 🎯 Visão Geral

Este guia descreve como configurar **Single Sign-On (SSO) SAML** entre o Microsoft Entra ID da **{MYCOMPANY}** (Identity Provider) e a aplicação **INDECX** (Service Provider), permitindo que usuários {MYCOMPANY} acessem a INDECX via login único.


## ✅ Pré-requisitos

### 🔑 Permissões Necessárias
- [x] Acesso de **administrador** ao portal Azure AD da {MYCOMPANY}
- [x] Permissões para criar e configurar **Enterprise Application**
- [x] Permissões para criar **usuários de teste**

### 📊 Dados Fornecidos pela INDECX
| Campo | Valor |
|-------|-------|
| **Entity ID (SP EntityID)** | `https://www.app-indecx.com/` |
| **Reply URL / ACS URL** | `https://indecx.com/v2/sso/{mycompany}/acs` |

### 📤 Dados a Obter do Azure AD
- [ ] IdP EntityID do Azure AD
- [ ] Login URL (Single Sign-On Service URL)
- [ ] Logout URL (Single Logout Service URL)
- [ ] Certificado de assinatura (base64)
- [ ] metadata.xml do IdP (federation metadata)

---

## ⚙️ Configuração Passo a Passo

### 1. Criar Enterprise Application SAML no Azure AD

#### 🌐 Acessar Portal Azure
1. Acesse o [Portal do Azure](https://portal.azure.com)
2. Entre com conta com privilégios de administrador no tenant da **{MYCOMPANY}**

#### 📱 Criar Nova Aplicação
1. No menu lateral → **Azure Active Directory**
2. Selecione **Enterprise applications**
3. Clique em **➕ New application**
4. Escolha **Create your own application**

#### ⚙️ Configurar Aplicação
```yaml
Nome: INDECX SSO MYCOMPANY
Tipo: "Integrate any other application you don't find in the gallery (Non-gallery)"
```

5. Clique em **Create**
6. Aguarde a criação e clique na aplicação criada

---

### 2. Configurar Single Sign-On (SSO) via SAML

#### 🔧 Configuração Básica SAML
1. No menu lateral → **Single sign-on**
2. Escolha o modo **SAML**
3. Na seção **Basic SAML Configuration** → **Edit**

```yaml
Identifier (Entity ID): https://www.app-indecx.com/
Reply URL (ACS URL): https://indecx.com/v2/sso/{mycompany}/acs
```

4. **Confirme** e **salve**

#### 📋 Obter Informações do IdP
Após salvar, anote os seguintes dados da seção **Set up INDECX SSO**:

| Campo | Descrição | Exemplo |
|-------|-----------|---------|
| **Login URL** | Single Sign-On Service URL | `https://login.microsoftonline.com/{tenant-id}/saml2` |
| **Azure AD Identifier** | IdP Entity ID | `https://sts.windows.net/{tenant-guid}/` |

#### 📜 Download de Certificados
Na seção **SAML Signing Certificate**:
- [ ] Download **Certificate (Base64)**
- [ ] Download **Federation Metadata XML**

> 💡 **Dica:** O arquivo metadata.xml contém todos os endpoints, EntityID e certificado em formato XML.

---

### 3. Obter as Informações "Dados {MYCOMPANY}" para Configurar na INDECX

#### 📦 Checklist de Dados para Envio

| ✅ | Item | Fonte |
|----|------|-------|
| [ ] | **EntityID do IdP** | Azure AD Identifier |
| [ ] | **SSO URL** | Login URL |
| [ ] | **Logout URL** | Logout URL (se disponível) |
| [ ] | **Certificado (Base64)** | Download Certificate |
| [ ] | **metadata.xml** | Download Federation Metadata |
| [ ] | **Usuário de teste** | Criado no Azure AD |
| [ ] | **Senha temporária** | Definida para teste |

---

### 4. Compartilhar com a Equipe INDECX e Configuração no Lado INDECX

#### 📧 Envio dos Dados
Encaminhe para a equipe INDECX:

```markdown
## Dados de Configuração SSO - {MYCOMPANY}

### 🔗 URLs e Identificadores
- **IdP EntityID:** [Azure AD Identifier]
- **SSO URL:** [Login URL]
- **Logout URL:** [Se disponível]

### 🏆 Certificados
- **Certificate (Base64):** [Anexar arquivo]
- **Federation Metadata XML:** [Anexar arquivo]

### 👤 Credenciais de Teste
- **Usuário:** [usuario.teste@mycompany.com]
- **Senha:** [senha_temporaria_123]
```

#### 🧪 Processo de Teste
1. **INDECX** configura ambiente produtivo com dados fornecidos
2. **Teste de login:**
   - Usuário acessa `https://v3.app-indecx.com/{mycompany}`
   - É redirecionado para SSO
   - Faz login com credenciais de teste
   - Recebe resposta SAML e é autenticado
   - É direcionado para página inicial da INDECX

---

## 📚 Documentação e Referências Microsoft

### 🔗 Links Oficiais

| 📖 Documento | 🔗 Link |
|--------------|---------|
| **Habilitar SSO SAML para Enterprise Application** | [Microsoft Learn](https://learn.microsoft.com/en-us/entra/identity/enterprise-apps/add-application-portal-setup-sso) |
| **Configuração de SAML com Microsoft Entra ID** | [Microsoft Learn](https://learn.microsoft.com/en-us/entra/identity/enterprise-apps/migrate-adfs-saml-based-sso) |
| **Gerenciar certificados de federação SAML** | [Microsoft Learn](https://learn.microsoft.com/en-us/entra/identity/enterprise-apps/tutorial-manage-certificates-for-federated-single-sign-on) |
| **Protocolo SAML de Single Sign-On** | [Microsoft Learn](https://learn.microsoft.com/en-us/entra/identity-platform/single-sign-on-saml-protocol) |
| **Metadata de federação Microsoft Entra** | [Microsoft Learn](https://learn.microsoft.com/en-us/entra/identity-platform/federation-metadata) |
| **Arquitetura de autenticação SAML** | [Microsoft Learn](https://learn.microsoft.com/en-us/entra/identity/architecture/auth-saml) |

---

### 📞 Suporte

Para problemas técnicos:
- **Microsoft Entra ID:** [Documentação oficial](https://learn.microsoft.com/en-us/entra/)
- **INDECX:** Entre em contato com a equipe de suporte INDECX

---

### 📝 Notas Importantes

> ⚠️ **Atenção:** Mantenha as credenciais de teste seguras e remova-as após a validação

> 🔒 **Segurança:** Todos os certificados devem ser armazenados de forma segura

> 🔄 **Manutenção:** Certificados SAML expiram (padrão: 3 anos). Configure alertas de renovação

---
