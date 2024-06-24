+++
title = "Bypassing Okta’s Passwordless MFA: Analysis"
date = 2024-06-23
[taxonomies]
tags = ["okta", "passwordless", "MFA bypass"]
+++

> Recognizing the vulnerabilities of MFA, the industry introduced passwordless authentication. This method eliminates the need for users to remember passwords, thereby reducing the risk of credential theft. Instead of “something you know,” users authenticate using “something you are,” such as a fingerprint or facial recognition. However, like all security measures, passwordless authentication is not immune to attacks.

> Okta FastPass is a cryptographic multi-factor authenticator that facilitates passwordless authentication for any SAML, OIDC, or WS-Fed applications within Okta. It functions as a device-bound authenticator and is compatible with various operating systems through the Okta Verify application.

> Okta Verify also supports biometric verification via Face ID or fingerprint recognition. If the user enables this option, an additional set of public/private keys is generated from the hardware TPM, with the public key sent to Okta’s servers. This biometric verification feature is termed by Okta as a “User Verification” factor, or “something you are.”

> Okta FastPass is designed to be a phishing-resistant factor. This authentication method prevents access if an Adversary-in-the-Middle authentication flow (like Evilginx’s) is detected.

{{ image(src="okta_passwordless.png", alt="okta passwordless") }}

> Is Passwordless Bulletproof? The short answer is no. Most implementations of passwordless device-bound and biometric MFA solutions generate cryptographic keys stored in the TPM. With the right access and permissions, these keys can be misused.

> OktaTerrify intercepts the token exchange between the attacker’s machine and Okta’s backend, emulating Okta FastPass. It also generates the OktaInk command lines to execute. OktaInk is executed on the victim’s machine to access the device-bound and user-verification factors, creating a valid FastPass token accepted by Okta’s backend.

From a red team perspective, this "something you are" verification factor is making phishing more difficult going forward as you will need to first compromise a system in order to extract a key from Okta Verify's database. I wish the post had more details around how it was reading the database in the first place. Is it not protected by using Windows Hello Enhanced Sign-In Security API?

This also means that phishing against Okta Fastpass won't be used as a method of initial access, it's more of persistence technique and potential lateral movement based on the user's application access.