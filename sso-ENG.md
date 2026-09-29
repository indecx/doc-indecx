# 🔐 SSO SAML Configuration Guide
### Microsoft Entra ID (Azure AD) ↔ INDECX Integration

[![Microsoft Entra ID](https://img.shields.io/badge/Microsoft%20Entra%20ID-0078D4?style=flat&logo=me](https://portal.azure.com)
[![SAML/img.shields.io/badge/SAML%202.0-FF6B35?style=flat&logo=xml&logoColor=white](https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=security)
[![SSO](https://img.shieldse/Single%20Sign--On-28A745?style=flat&logo=key&logoColor=white](https://en.wikipedia.org/wiki/Single_sign-on)

---

## 📋 Table of Contents

- #-overview
- #-prerequisites
- [-step-by-step-configuration
  - [1. Create Enterprise Application](#1-create-saml-enterprise-application-SSO](#2-configure-single-sign-on-sso-via-rmation for INDECX](#3-obtain-mycompany-  - [4. Share with INDECX](#4-share-with-the-indecx-team-and-configuration-on-the-indecx-sideation-and-references
- #-troubleshooting

---

## 🎯 Overview

This guide describes how to configure **SAML Single Sign-On (SSO)** between **{MYCOMPANY}** Microsoft Entra ID (Identity Provider) and the **INDECX** application (Service Provider), allowing {MYCOMPANY} users to access INDECX via single sign-on.

## ✅ Prerequisites

### 🔑 Required Permissions
- [x] **Administrator** access to the {MYCOMPANY} Azure AD portal
- [x] Permissions to create and configure **Enterprise Applications**
- [x] Permissions to create **test users**

### 📊 Information Provided by INDECX
| Field | Value |
|-------|-------|
| **Entity ID (SP EntityID)** | `https://v3.app-indecx.com/` |
| **Reply URL / ACS URL** | `https://indecx.com/v2/sso/{mycompany}/acs` |

### 📤 Information to Obtain from Azure AD
- [ ] Azure AD IdP EntityID
- [ ] Login URL (Single Sign-On Service URL)
- [ ] Logout URL (Single Logout Service URL)
- [ ] Signing certificate (Base64)
- [ ] IdP metadata.xml (federation metadata)

---

## ⚙️ Step-by-Step Configuration

### 1. Create SAML Enterprise Application in Azure AD

#### 🌐 Access the Azure Portal
1. Go to the [Azure Portal](https://portal.azure.com)
2. Sign in with an account that has administrator privileges in the **{MYCOMPANY}** tenant

#### 📱 Create a New Application
1. In the left menu → **Azure Active Directory**
2. Select **Enterprise applications**
3. Click **➕ New application**
4. Choose **Create your own application**

#### ⚙️ Configure the Application

```yaml
Name: INDECX SSO MYCOMPANY
Type: "Integrate any other application you don't find in the gallery (Non-gallery)"
```

5. Click **Create**
6. Wait for the application to be created and open the newly created application

---

### 2. Configure Single Sign-On (SSO) via SAML

#### 🔧 Basic SAML Configuration

1. In the left menu → **Single sign-on**
2. Choose the **SAML** mode
3. In the **Basic SAML Configuration** section → **Edit**

```yaml
Identifier (Entity ID): https://v3.app-indecx.com/
Reply URL (ACS URL): https://indecx.com/v2/sso/{mycompany}/acs
```

4. **Confirm** and **save**

#### 📋 Obtain IdP Information

After saving, note the following information from the **Set up INDECX SSO** section:

| Field | Description | Example |
|-------|-------------|---------|
| **Login URL** | Single Sign-On Service URL | `https://login.microsoftonline.com/{tenant-id}/saml2` |
| **Azure AD Identifier** | IdP Entity ID | `https://sts.windows.net/{tenant-guid}/` |

#### 📜 Download Certificates

In the **SAML Signing Certificate** section:

- [ ] Download **Certificate (Base64)**
- [ ] Download **Federation Metadata XML**

> 💡 **Tip:** The metadata.xml file contains all endpoints, EntityID information, and the certificate in XML format.

---

### 3. Obtain "{MYCOMPANY} Data" Information to Configure in INDECX

#### 📦 Information Submission Checklist

| ✅ | Item | Source |
|----|------|--------|
| [ ] | **IdP EntityID** | Azure AD Identifier |
| [ ] | **SSO URL** | Login URL |
| [ ] | **Logout URL** | Logout URL (if available) |
| [ ] | **Certificate (Base64)** | Certificate Download |
| [ ] | **metadata.xml** | Federation Metadata Download |
| [ ] | **Test User** | Created in Azure AD |
| [ ] | **Temporary Password** | Defined for testing |

---

### 4. Share with the INDECX Team and Configuration on the INDECX Side

#### 📧 Sending the Information

Forward the following to the INDECX team:

```markdown
## SSO Configuration Information - {MYCOMPANY}

### 🔗 URLs and Identifiers
- **IdP EntityID:** [Azure AD Identifier]
- **SSO URL:** [Login URL]
- **Logout URL:** [If available]

### 🏆 Certificates
- **Certificate (Base64):** [Attach file]
- **Federation Metadata XML:** [Attach file]

### 👤 Test Credentials
- **User:** [test.user@mycompany.com]
- **Password:** [temporary_password_123]
```

#### 🧪 Testing Process

1. **INDECX** configures the production environment using the provided information
2. **Login test:**
   - User accesses `https://v3.app-indecx.com/{mycompany}`
   - Is redirected to SSO
   - Signs in using the test credentials
   - Receives the SAML response and is authenticated
   - Is redirected to the INDECX home page

---

## 📚 Microsoft Documentation and References

### 🔗 Official Links

| 📖 Document | 🔗 Link |
|------------|---------|
| **Enable SAML SSO for Enterprise Application** | [Microsoft Learn](https://learn.microsoft.com/en-us/entra/identity/enterprise-apps/add-application-portal-setup-sso) |
| **SAML Configuration with Microsoft Entra ID** | [Microsoft Learn](https://learn.microsoft.com/en-us/entra/identity/enterprise-apps/migrate-adfs-saml-based-sso) |
| **Manage SAML Federation Certificates** | [Microsoft Learn](https://learn.microsoft.com/en-us/entra/identity/enterprise-apps/tutorial-manage-certificates-for-federated-single-sign-on) |
| **SAML Single Sign-On Protocol** | [Microsoft Learn](https://learn.microsoft.com/en-us/entra/identity-platform/single-sign-on-saml-protocol) |
| **Microsoft Entra Federation Metadata** | [Microsoft Learn](https://learn.microsoft.com/en-us/entra/identity-platform/federation-metadata) |
| **SAML Authentication Architecture** | [Microsoft Learn](https://learn.microsoft.com/en-us/entra/identity/architecture/auth-saml) |

---

### 📞 Support

For technical issues:

- **Microsoft Entra ID:** https://learn.microsoft.com/en-us/entra/
- **INDECX:** Contact the INDECX support team

---

### 📝 Important Notes

> ⚠️ **Attention:** Keep test credentials secure and remove them after validation.

> 🔒 **Security:** All certificates must be stored securely.

> 🔄 **Maintenance:** SAML certificates expire (default: 3 years). Configure renewal alerts.

---
