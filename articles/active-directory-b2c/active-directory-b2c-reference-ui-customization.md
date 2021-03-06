---
title: "Azure Active Directory B2C : outil d’assistance de personnalisation d’interface utilisateur | Microsoft Docs"
description: "Une rubrique sur les fonctionnalités de personnalisation de l’interface utilisateur (UI) dans Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: 99f5a391-5328-471d-a15c-a2fafafe233d
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
translationtype: Human Translation
ms.sourcegitcommit: e37c48d6c92a8a2cd480458abdff0a3a1ca9338f
ms.openlocfilehash: bcb91660500484210c342a84595386ebb5e0bbbd


---
# <a name="azure-active-directory-b2c-customize-the-azure-ad-b2c-user-interface-ui"></a>Azure Active Directory B2C : personnalisation de l’interface utilisateur Azure AD B2C
L’expérience utilisateur est primordiale dans une application utilisée par les consommateurs. Elle fait la différence entre une bonne application et une excellente, et entre les consommateurs simplement actifs et ceux qui sont réellement engagés. Azure Active Directory (Azure AD) B2C vous permet de personnaliser les pages d’inscription, de connexion (*voir la remarque ci-dessous*), d’édition de profil de l’utilisateur et de réinitialisation de mot de passe avec le contrôle pixel-perfect.

> [!NOTE]
> Actuellement, les pages d’inscription au compte local, les pages de réinitialisation de mot de passe associées ainsi que les courriers électroniques de vérification ne sont personnalisables qu’avec la [fonction de personnalisation de la société](../active-directory/active-directory-add-company-branding.md) et non par le biais des mécanismes décrits dans cet article.
> 
> 

Cet article portera sur les éléments suivants :

* La fonctionnalité de personnalisation de l’interface utilisateur de page.
* Un outil qui vous permettra de tester la fonctionnalité de personnalisation d’interface utilisateur de page à l’aide de notre exemple de contenu.
* Les éléments d’interface utilisateur de base dans chaque type de page.
* Meilleures pratiques lors de l’exercice de cette fonctionnalité.

## <a name="the-page-ui-customization-feature"></a>La fonctionnalité de personnalisation de l’interface utilisateur de page
Avec la fonctionnalité de personnalisation de l’interface utilisateur de page, vous pouvez personnaliser l’apparence des pages d’inscription, de connexion, de réinitialisation du mot de passe et de modification de profil du consommateur (en configurant des [stratégies](active-directory-b2c-reference-policies.md)). Vos clients bénéficieront d’une expérience transparente lors de la navigation entre votre application et les pages fournies par Azure AD B2C.

Contrairement aux autres services où les options de l’interface sont limitées ou ne sont disponibles que via les API, Azure AD B2C utilise une approche moderne (et plus simple) de la personnalisation de l’interface utilisateur de page.

Voici comment cela fonctionne : Azure AD B2C exécute le code dans l’explorateur client et utilise une approche moderne appelée [partage de ressources cross-origin (CORS)](http://www.w3.org/TR/cors/) pour charger le contenu depuis une URL spécifiée au préalable dans une stratégie. Vous pouvez spécifier des URL différentes pour différentes pages. Le code fusionne des éléments de l’interface utilisateur d’Azure AD B2C et le contenu chargé depuis votre URL, et affiche la page pour le consommateur. Procédure à suivre :

1. Créer un contenu HTML5 correct avec un élément `<div id="api"></div>` (obligatoirement vide) situé dans `<body>`. Cet élément marque l’endroit où le contenu Azure AD B2C est inséré.
2. Héberger votre contenu sur un point de terminaison HTTPS (avec CORS activé).
3. Modifier le style des éléments d’interface utilisateur dans lesquels sera incorporé Azure AD B2C.

## <a name="test-out-the-ui-customization-feature"></a>Tester la fonctionnalité de personnalisation de l’interface
Si vous souhaitez tester la fonctionnalité de personnalisation de l’interface utilisateur à l’aide de notre exemple de contenu HTML et CSS, nous vous proposons [un outil d’assistance simple](active-directory-b2c-reference-ui-customization-helper-tool.md) permettant de télécharger et configurer votre exemple de contenu sur votre instance Azure Blob Storage.

> [!NOTE]
> Vous pouvez héberger le contenu de votre interface utilisateur sur tous types d’emplacements : serveurs web, CDN, AWS S3, systèmes de partage de fichiers, etc. Tout est possible à condition que le contenu soit hébergé sur un point de terminaison HTTPS disponible publiquement (avec CORS activé). Nous utilisons Azure Blob Storage à titre indicatif uniquement.
> 
> 

### <a name="the-most-basic-html-content-for-testing"></a>Contenu HTML de base à des fins de test
Vous trouverez ci-dessous un contenu HTML élémentaire que vous pouvez utiliser pour tester cette fonctionnalité. Utilisez le même outil d’assistance que celui fourni précédemment pour télécharger et configurer ce contenu sur votre instance Azure Blob Storage. Vous pouvez ensuite vérifier l’affichage et le fonctionnement des boutons et champs de formulaire de base (non stylisés) sur chaque page.

```HTML

<!DOCTYPE html>
<html>
    <head>
        <title>!Add your title here!</title>
    </head>
    <body>
        <div id="api"></div>    <!-- IMP: This element is intentionally empty; don't enter anything here -->
    </body>
</html>

```

## <a name="the-core-ui-elements-in-each-type-of-page"></a>Éléments d’interface utilisateur de base dans chaque type de page
Les sections suivantes présentent des exemples de fragments HTML5 qu’Azure AD B2C fusionne dans l’élément `<div id="api"></div>` situé dans votre contenu. **N’insérez pas ces fragments dans votre contenu HTML5.**  Le service Azure AD B2C les insère au moment de l’exécution. Utilisez ces exemples pour concevoir vos propres feuilles de style.

### <a name="azure-ad-b2c-content-inserted-into-the-identity-provider-selection-page"></a>Contenu Azure AD B2C inséré dans la « page de sélection du fournisseur d’identité »
Cette page contient une liste de fournisseurs d’identité parmi lesquels l’utilisateur peut faire son choix à l’inscription ou à la connexion. Il s’agit de fournisseurs d’identité sociale tels que Facebook et Google + ou de comptes locaux (basés sur une adresse de messagerie ou un nom d’utilisateur).

```HTML

<div id="api" data-name="IdpSelections">
    <div class="intro">
         <p>Sign up</p>
    </div>

    <div>
        <ul>
            <li>
                <button class="accountButton" id="FacebookExchange">Facebook</button>
            </li>
            <li>
                <button class="accountButton" id="GoogleExchange">Google+</button>
            </li>
            <li>
                <button class="accountButton" id="SignUpWithLogonEmailExchange">Email</button>
            </li>
        </ul>
    </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-local-account-sign-up-page"></a>Contenu Azure AD B2C inséré dans la « page d’inscription à un compte local »
Cette page contient un formulaire d’inscription que l’utilisateur doit renseigner lors de l’inscription pour un compte local basé sur une adresse électronique ou un nom d’utilisateur. Le formulaire peut contenir différentes commandes de saisie telles que la zone de saisie de texte, celle du mot de passe, le bouton radio, les cases du menu déroulant à sélection unique et la sélection de plusieurs cases à cocher.

```HTML

<div id="api" data-name="SelfAsserted">
    <div class="intro">
        <p>Create your account by providing the following details</p>
    </div>

    <div id="attributeVerification">
        <div class="errorText" id="passwordEntryMismatch" style="display: none;">The password entry fields do not match. Please enter the same password in both fields and try again.</div>
        <div class="errorText" id="requiredFieldMissing" style="display: none;">A required field is missing. Please fill out all required fields and try again.</div>
        <div class="errorText" id="fieldIncorrect" style="display: none;">One or more fields are filled out incorrectly. Please check your entries and try again.</div>
        <div class="errorText" id="claimVerificationServerError" style="display: none;"></div>
        <div class="attr" id="attributeList">
            <ul>
                <li>
                    <div class="attrEntry validate">
                        <div>
                            <div class="verificationInfoText" id="email_intro" style="display: inline;">Verification is necessary. Please click Send button.</div>
                            <div class="verificationInfoText" id="email_info" style="display:none">Verification code has been sent to your inbox. Please copy it to the input box below.</div>
                            <div class="verificationSuccessText" id="email_success" style="display:none">E-mail address verified. You can now continue.</div>
                            <div class="verificationErrorText" id="email_fail_retry" style="display:none">Incorrect code, try again.</div>
                            <div class="verificationErrorText" id="email_fail_no_retry" style="display:none">Exceeded number of retries you need to send new code.</div>
                            <div class="verificationErrorText" id="email_fail_server" style="display:none">Server error, please try again</div>
                            <div class="verificationErrorText" id="email_incorrect_format" style="display:none">Incorect format.</div>
                        </div>

                    <div class="helpText show">This information is required</div>
                        <label>Email</label>
                        <input id="email" class="textInput" type="text" placeholder="Email" required="" autofocus=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Email address that can be used to contact you.');" class="tiny">What is this?</a>

                    <div class="buttons verify" claim_id="email">
                        <div id="email_ver_wait" class="working" style="display: none;"></div>
                            <label id="email_ver_input_label" for="email_ver_input" style="display: none;">Verification code</label>
                            <input id="email_ver_input" type="text" placeholder="Verification code" style="display:none">
                            <button id="email_ver_but_send" class="sendButton" type="button" style="display: inline;">Send verification code</button>
                            <button id="email_ver_but_verify" class="verifyButton" type="button" style="display:none">Verify code</button>
                            <button id="email_ver_but_resend" class="sendButton" type="button" style="display:none">Send new code</button>
                            <button id="email_ver_but_edit" class="editButton" type="button" style="display:none">Change e-mail</button>
                            <button id="email_ver_but_default" class="defaultButton" type="button" style="display:none">Default</button>
                        </div>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">8-16 characters, containing 3 out of 4 of the following: Lowercase characters, uppercase characters, digits (0-9), and one or more of the following symbols: @ # $ % ^ &amp; * - _ + = [ ] { } | \ : ' , ? / ` ~ " ( ) ; .This information is required</div>
                        <label>Enter password</label>
                        <input id="password" class="textInput" type="password" placeholder="Enter password" pattern="^((?=.*[a-z])(?=.*[A-Z])(?=.*\d)|(?=.*[a-z])(?=.*[A-Z])(?=.*[^A-Za-z0-9])|(?=.*[a-z])(?=.*\d)(?=.*[^A-Za-z0-9])|(?=.*[A-Z])(?=.*\d)(?=.*[^A-Za-z0-9]))([A-Za-z\d@#$%^&amp;*\-_+=[\]{}|\\:',?/`~&quot;();!]|\.(?!@)){8,16}$" title="8-16 characters, containing 3 out of 4 of the following: Lowercase characters, uppercase characters, digits (0-9), and one or more of the following symbols: @ # $ % ^ &amp; * - _ + = [ ] { } | \ : ' , ? / ` ~ &quot; ( ) ; ." required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Enter password');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"> This information is required</div>
                        <label>Reenter password</label>
                        <input id="reenterPassword" class="textInput" type="password" placeholder="Reenter password" pattern="^((?=.*[a-z])(?=.*[A-Z])(?=.*\d)|(?=.*[a-z])(?=.*[A-Z])(?=.*[^A-Za-z0-9])|(?=.*[a-z])(?=.*\d)(?=.*[^A-Za-z0-9])|(?=.*[A-Z])(?=.*\d)(?=.*[^A-Za-z0-9]))([A-Za-z\d@#$%^&amp;*\-_+=[\]{}|\\:',?/`~&quot;();!]|\.(?!@)){8,16}$" title=" " required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Reenter password');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">This information is required</div>
                        <label>Name</label>
                        <input id="displayName" class="textInput" type="text" placeholder="Name" required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Your display name.');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>Gender</label>
                        <input id="extension_Gender_F" name="extension_Gender" type="radio" value="F" autofocus="">
                        <label for="extension_Gender_F">Female</label>
                        <input id="extension_Gender_M" name="extension_Gender" type="radio" value="M">
                        <label for="extension_Gender_M">Male</label>
                        <a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>Loyalty number</label>
                        <input id="extension_MemNum" class="textInput" type="text" placeholder="Loyalty number"><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Membership number');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>State</label>
                        <select class="dropdown_single" id="state">
                            <option style="display:none" disabled="disabled" value="placeholder" selected="">State</option>
                            <option value="WA">Washington</option>
                            <option value="NY">New York</option>
                            <option value="CA">California</option>
                        </select>
                        <a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Your residential state or province.');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">This information is required</div>
                        <label>Zip code</label>
                        <input id="postalCode" class="textInput" type="text" placeholder="Zip code" required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('The postal code of your address.');" class="tiny">What is this?</a>
                    </div>
                </li>
            </ul>
        </div>
        <div class="buttons"> <button id="continue" disabled="">Create</button> <button id="cancel">Cancel</button></div>
    </div>
    <div class="verifying-modal">
        <div class="preloader"> <img src="https://login.microsoftonline.com/static/img/win8loader.gif" alt="Please wait"></div>
        <div id="verifying_blurb"></div>
    </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-social-account-sign-up-page"></a>Contenu Azure AD B2C inséré dans la « page d’inscription à un compte social »
Cette page contient un formulaire d’inscription que le consommateur doit remplir lors de l’inscription à l’aide d’un compte existant à partir d’un fournisseur d’identité sociale tel que Facebook ou Google +. Cette page est similaire à la page d’inscription à un compte local (affiché dans la section précédente) à l’exception des champs de saisie de mot de passe.

### <a name="azure-ad-b2c-content-inserted-into-the-unified-sign-up-or-sign-in-page"></a>Contenu Azure AD B2C inséré dans la « page d’inscription ou de connexion unifiée »
Cette page gère l’inscription et la connexion des consommateurs, qui peuvent utiliser les fournisseurs d’identité des réseaux sociaux, tels que Facebook ou Google+, ou des comptes locaux.

```HTML

<div id="api" data-name="Unified">
        <div class="social" role="form">
               <div class="intro">
                       <h2>Sign in with your social account</h2>
               </div>
               <div class="options">
                       <div><button class="accountButton firstButton" id="MicrosoftAccountExchange" tabindex="1">msa</button></div>
                       <div><button class="accountButton" id="FacebookExchange" tabindex="1">fb</button></div>
               </div>
        </div>
        <div class="divider">
               <h2>OR</h2>
        </div>
        <div class="localAccount" role="form">
               <div class="intro">
                       <h2>Sign in with your existing account</h2>
               </div>
               <div class="error pageLevel" aria-hidden="true" style="display: none;">
                       <p role="alert"></p>
               </div>
               <div class="entry">
                       <div class="entry-item">
                               <label for="logonIdentifier">Email Address</label> 
                               <div class="error itemLevel" aria-hidden="true" style="display: none;">
                                      <p role="alert"></p>
                               </div>
                               <input type="email" id="logonIdentifier" name="LogonIdentifier" pattern="^[a-zA-Z0-9.!#$%&amp;’*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$" placeholder="LogonIdentifier" value="" tabindex="1">
                       </div>
                       <div class="entry-item">
                               <div class="password-label"> <label for="password">Password</label><a id="forgotPassword" tabindex="2">Forgot your password?</a></div>
                               <div class="error itemLevel" aria-hidden="true" style="display: none;">
                                      <p role="alert"></p>
                               </div>
                               <input type="password" id="password" name="Password" placeholder="Password" tabindex="1">
                       </div>
                       <div class="working"></div>
                       <div class="buttons"> <button id="next" tabindex="1">Sign in</button> </div>
               </div>
               <div class="divider">
                       <h2>OR</h2>
               </div>
               <div class="create">
                       <p>Don't have an account?<a id="createAccount" tabindex="1">Sign up now</a> </p>
               </div>
        </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-multi-factor-authentication-page"></a>Contenu Azure AD B2C inséré dans la « page d’authentification multifacteur »
Cette page permet aux utilisateurs de vérifier leurs numéros de téléphone (par voie textuelle ou vocale) au cours de l’inscription ou de la connexion.

```HTML

<div id="api" data-name="Phonefactor">
    <div id="phonefactor_initial">
        <div class="intro">
            <p>Enter a number below that we can send a code via SMS or phone to authenticate you.</p>
        </div>
        <div class="errorText" id="errorMessage" style="display:none"></div>
        <div class="phoneEntry" id="phoneEntry">
            <div class="phoneNumber">
                <select id="countryCode" style="display:inline-block">
                    <option value="+93">Afghanistan (+93)</option>
                    <!-- Not all country codes listed -->
                    <option value="+44">United Kingdom (+44)</option>
                    <option value="+1" selected="">United States (+1)</option>
                    <!-- Not all country codes listed -->
                </select>
            </div>
            <div class="phoneNumber">
                <input type="text" id="localNumber" style="display:inline-block" placeholder="Phone number">
            </div>
        </div>
        <div id="codeVerification" style="display:none">
            <div>
                <p>Enter your verification code below, or <button id="retryCode" class="textButton">send a new code</button></p>
                <input type="text" id="verificationCode" placeholder="Verification code">
            </div>
        </div>
        <div class="buttons">
            <button id="verifyCode" class="sendInitialCodeButton">Send Code</button>
            <button id="verifyPhone" style="display:inline-block">Call Me</button>
            <button id="cancel" style="display:inline-block">Cancel</button>
        </div>
    </div>
    <div class="dialing-modal">
        <div class="preloader"> <img src="https://login.microsoftonline.com/static/img/win8loader.gif" alt="Please wait"></div>
        <div id="dialing_blurb"></div><div id="dialing_number"></div>
    </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-error-page"></a>Contenu Azure AD B2C inséré dans la « page d’erreur »
```HTML

<div id="api" class="error-page-content" data-name="GlobalException">
    <h2>Sorry, but we're having trouble signing you in.</h2>
    <div class="error-page-help">We track these errors automatically, but if the problem persists feel free to contact us. In the meantime, please try again.</div>
    <div class="error-page-messagedetails">Your administrator hasn't provided any contact details.</div>
    <div class="error-page-messagedetails">
        <div class="error-page-correlationid">Correlation ID:1c4f0397-c6e4-4afe-bf74-42f488f2f15f</div>
        <div>Timestamp:2015-09-14 23:22:35Z</div>
        <div class="error-page-detail">AADB2C90065: A B2C client-side error 'Access is denied.' has occurred requesting the remote resource.</div>
    </div>
</div>

```

## <a name="things-to-remember-when-building-your-own-content"></a>Points à retenir lors de la création de votre propre contenu
Si vous envisagez d’utiliser la fonctionnalité de personnalisation d’interface utilisateur de page, veuillez consulter les meilleures pratiques suivantes :

* Ne copiez pas le contenu par défaut Azure AD B2C et n’essayez pas de le modifier. Il est préférable de créer votre contenu HTML5 à partir de zéro et d’utiliser le contenu par défaut comme référence.
* Dans toutes les pages (à l’exception des pages d’erreur) prises en charge par les stratégies d’inscription, de connexion et de modification de profil, les feuilles de style que vous fournissez devront remplacer les feuilles de style par défaut que nous ajoutons à ces pages dans les fragments <head> . Dans toutes les pages prises en charge par les stratégies d’inscription, de connexion ou de réinitialisation du mot de passe, ainsi que dans les pages d’erreur de toutes les stratégies, vous devrez définir vous-même les éléments de style.
* Pour des raisons de sécurité, nous ne vous permettons pas d’inclure du langage JavaScript dans votre contenu. La plupart des éléments dont vous avez besoin sont disponibles et prêts à l’emploi. Dans le cas contraire, utilisez [User Voice](http://feedback.azure.com/forums/169401-azure-active-directory) pour demander de nouvelles fonctionnalités.
* Versions de navigateur prises en charge :
  * Internet Explorer 11
  * Internet Explorer 10
  * Internet Explorer 9 (limité)
  * Internet Explorer 8 (limité)
  * Google Chrome 43.0
  * Google Chrome 42.0
  * Mozilla Firefox 38.0
  * Mozilla Firefox 37.0




<!--HONumber=Jan17_HO1-->


